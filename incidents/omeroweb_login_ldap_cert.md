omeroweb logs
```
2023-08-01 16:05:47,465 ERROR [                 omeroweb.webadmin.views] (proc.13872) forgotten_password():396 object #0 (::omero::cmd::ERR)
2023-08-01 17:05:26,878 ERROR [                 omeroweb.webadmin.views] (proc.16425) forgotten_password():396 object #0 (::omero::cmd::ERR)
2023-08-01 17:05:47,419 ERROR [                 omeroweb.webadmin.views] (proc.00757) forgotten_password():396 object #0 (::omero::cmd::ERR)
```

LDAP people put on a new cert and didn't tell anyone, apparently this has happened before

Created ticket

Possible work-around is to turn off SSL LDAP:
`omero config set omero.ldap.urls ldap://ldap.jax.org:3268`

But instead, just run this to re-create keystore (first move omero_keystore to omero_keystore_old or something)
```
openssl s_client -connect ldaps.jax.org:3269 -prexit < /dev/null | openssl x509 -outform PEM | keytool -import -alias ldap -storepass omero_storepass -keystore /local/omero_app/omero_keystore -noprompt
```