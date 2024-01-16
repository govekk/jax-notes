# readonly server pods erroring every 45 days or so
`kubectl describe pod omero-readonly-server-xxxx`
```
Status:           Failed
Reason:           Evicted
Message:          Pod ephemeral local storage usage exceeds the total limit of containers 4Gi
```
- too many logs??

```
omero-server@omero-readonly-server-f97f694d8-z7bpj:/$ du --max-depth 1 -h 
6.8M	./sbin
418M	./tmp
2.7M	./etc * access error for /etc/ssl/private
0	    ./proc * some access errors
23M	    ./lib
4.0K	./boot
4.0K	./media
44K	    ./home
7.0M	./bin
1.7G	./opt
4.0K	./root * access error for /root
0	    ./dev
4.0K	./lib64
17M	    ./var * access serrors for two dirs in /var/cache
1.1G	./usr
0	    ./sys
4.0K	./mnt
4.0K	./srv
8.0K	./run
49G	    ./local
16T	    ./hyperfile
20K	    ./startup
16T	.
```

/opt/omero/server/OMERO.server is currently at 1.7G, likely will grow cause that's where OMERO logs are stored. Could put that on Filestore?

Note: both my sandbox project and pub have cluster warnings about failing to create "container-watcher-" around Dec 7 so that might have just been a Google error

# Other pod state
https://github.com/kubernetes/kube-state-metrics
+ Prometheus