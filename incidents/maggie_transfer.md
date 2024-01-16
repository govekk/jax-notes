```
ssh govekk_pa@ctomero01lp.jax.org
sudo su svc-omero
svc # shortcut command that opens omerovenv
screen
omero chown 453 Experimenter:504 --report --dry-run
```

```
Included objects
  Instrument:31110-31201,55389-55412,65884-65907,68405,68406,83213-83322,84636-84645,125268-125357,130091,130092,130148,130834,130835,132347-132370,144496-144498
  Microscope:31110-31201,55378-55401,65875-65898,68396,68397,83213-83322,84636-84645,125068-125157,129891,129892,129948,130634,130635,132147-132170,144269-144271
  Objective:31110-31201,55435-55458,65979-66002,68500,68501,83313-83422,84736-84745,125368-125457,130191,130192,130248,130934,130935,132447-132470,144596-144598
  ObjectiveSettings:60258-60533,94730-94801,118923-118994,124514-124519,144159-144488,146644-146673,188902-189171,194109-194114,194176-194178,195460-195465,197105-197176,212921-212929
  CommentAnnotation:63930,63931,63933,63935,63937,63939,63941,63943,63945,63947,63949,63951,63953,63955,63957,63959,63961,63963,63965,63967,63969,63971,63973,63975,63977,63979,63981,63983,63985,63987,63989,63991,63993,63995,63997,63999,64001,64003,64005,64007,64009,64011,64013,64015,64017,64019,64021,64023,64025,64027,64029,64031,64033,64035,64037,64039,64041,64043,64045,64047,64049,64051,64053,64055,64057,64059,64061,64063,64065,64067,64069,64071,64073,64075,64077,64079,64081,64083,64085,64087,64089,64091,64093,64095,64097,64099,64101,64103,64105,64107,64109,64111,64113,79512,79514,79516,79518,79520,79522,79524,79526,79528,79530,79532,79534,79536,79538,79540,79542,79544,79546,79548,79550,79552,79554,79556,79558,99694,99696,99698,99700,99702,99704,99706,99708,99710,99712,99714,99716,99718,99720,99722,99724,99726,99728,99730,99732,99734,99736,99738,99740,106599,106601,114807,114809,114811,114813,114815,114817,114819,114821,114823,114825,114827,114829,114831,114833,114835,114837,114839,114841,114843,114845,114847,114849,114851,114853,114855,114857,114859,114861,114863,114865,114867,114869,114871,114873,114875,114877,114879,114881,114883,114885,114887,114889,114891,114893,114895,114897,114899,114901,114903,114905,114907,114909,114911,114913,114915,114917,114919,114921,114923,114925,114927,114929,114931,114933,114935,114937,114939,114941,114943,114945,114947,114949,114951,114953,114955,114957,114959,114961,114963,114965,114967,114969,114971,114973,114975,114977,114979,114981,114983,114985,114987,114989,114991,114993,114995,114997,114999,115001,115003,115005,115007,115009,115011,115013,115015,115017,115019,115021,115023,115025,116779,116781,116783,116785,116787,116789,116791,116793,116795,116797,123331,123333,123335,123337,123339,123341,123343,123345,123347,123349,123351,123353,123355,123357,123359,123361,123363,123365,123367,123369,123371,123373,123375,123377,123379,123381,123383,123385,123387,123389,123391,123393,123395,123397,123399,123401,123403,123405,123407,123409,123411,123413,123415,123417,123419,123421,123423,123425,123427,123429,123431,123433,123435,123437,123439,123441,123443,123445,123447,123449,123451,123453,123455,123457,123459,123461,123463,123465,123467,123469,123471,123473,123475,123477,123479,123481,123483,123485,123487,123489,123491,123493,123495,123497,123499,123501,123503,123505,123507,123509,123838,123840,123863,125044,125046,125367,125369,125371,125373,125375,125377,125379,125381,125383,125385,125387,125389,125391,125393,125395,125397,125399,125401,125403,125405,125407,125409,125411,125413,133541,133543,133545
  FileAnnotation:117362,117366,117399,117401,117402
  FilesetAnnotationLink:18556-18648,24609-24632,32131-32154,34262,34263,37252-37361,37788-37797,41330-41419,41577,41578,41587,42150,42151,42244-42267,45517-45519
  ImageAnnotationLink:82871-83146,130387-130458,160808-160879,169575-169580,191963-192292,195293-195322,207043,207044,207104-207123,207129-207135,207140-207144,252895-253164,258400-258405,258475-258477,259994-259999,261730-261801,284062-284070
  MapAnnotation:63932,63934,63936,63938,63940,63942,63944,63946,63948,63950,63952,63954,63956,63958,63960,63962,63964,63966,63968,63970,63972,63974,63976,63978,63980,63982,63984,63986,63988,63990,63992,63994,63996,63998,64000,64002,64004,64006,64008,64010,64012,64014,64016,64018,64020,64022,64024,64026,64028,64030,64032,64034,64036,64038,64040,64042,64044,64046,64048,64050,64052,64054,64056,64058,64060,64062,64064,64066,64068,64070,64072,64074,64076,64078,64080,64082,64084,64086,64088,64090,64092,64094,64096,64098,64100,64102,64104,64106,64108,64110,64112,64114,79513,79515,79517,79519,79521,79523,79525,79527,79529,79531,79533,79535,79537,79539,79541,79543,79545,79547,79549,79551,79553,79555,79557,79559,99695,99697,99699,99701,99703,99705,99707,99709,99711,99713,99715,99717,99719,99721,99723,99725,99727,99729,99731,99733,99735,99737,99739,99741,106600,106602,114808,114810,114812,114814,114816,114818,114820,114822,114824,114826,114828,114830,114832,114834,114836,114838,114840,114842,114844,114846,114848,114850,114852,114854,114856,114858,114860,114862,114864,114866,114868,114870,114872,114874,114876,114878,114880,114882,114884,114886,114888,114890,114892,114894,114896,114898,114900,114902,114904,114906,114908,114910,114912,114914,114916,114918,114920,114922,114924,114926,114928,114930,114932,114934,114936,114938,114940,114942,114944,114946,114948,114950,114952,114954,114956,114958,114960,114962,114964,114966,114968,114970,114972,114974,114976,114978,114980,114982,114984,114986,114988,114990,114992,114994,114996,114998,115000,115002,115004,115006,115008,115010,115012,115014,115016,115018,115020,115022,115024,115026,116780,116782,116784,116786,116788,116790,116792,116794,116796,116798,123332,123334,123336,123338,123340,123342,123344,123346,123348,123350,123352,123354,123356,123358,123360,123362,123364,123366,123368,123370,123372,123374,123376,123378,123380,123382,123384,123386,123388,123390,123392,123394,123396,123398,123400,123402,123404,123406,123408,123410,123412,123414,123416,123418,123420,123422,123424,123426,123428,123430,123432,123434,123436,123438,123440,123442,123444,123446,123448,123450,123452,123454,123456,123458,123460,123462,123464,123466,123468,123470,123472,123474,123476,123478,123480,123482,123484,123486,123488,123490,123492,123494,123496,123498,123500,123502,123504,123506,123508,123510,123839,123841,123864,125045,125047,125368,125370,125372,125374,125376,125378,125380,125382,125384,125386,125388,125390,125392,125394,125396,125398,125400,125402,125404,125406,125408,125410,125412,125414,133542,133544,133546
  TagAnnotation:117390-117396
  Dataset:2012,2486,2577,2953,3288,3324,3360,3395,3767
  DatasetImageLink:78085-78360,114432-114503,142842-142913,149014-149019,169109-169438,171594-171623,213705-213974,219296-219304,220841-220846,222564-222635,240140-240148
  Project:906,1052
  ProjectDatasetLink:2577,3007-3009,3285,3321,3357,3392,3766
  Channel:173177-173820,268811-268978,329736-329903,344091-344104,407029-407798,413494-413563,1015827-1016456,1025564-1025577,1025755-1025761,1029738-1029751,1034162-1034329,1088906-1088926
  Image:67664-67939,103093-103164,127982-128053,134113-134118,156609-156938,159094-159123,361338-361607,366551-366556,366618-366620,368145-368150,369790-369861,386826-386834
  LogicalChannel:173177-173820,268811-268978,329730-329897,344071-344084,399729-400498,406194-406263,535927-536556,545664-545677,545855-545861,549838-549851,554262-554429,609006-609026
  OriginalFile:75903-75905,75908,75911,75914,75917,75920,75923,75926,75929,75932,75935,75938,75941,75944-76222,99993-100066,128766-128839,138245-138251,273274-273605,275857-275887,341566,363188,459459,459461,459462,762290-762561,763688-763694,763755-763758,765822-765829,766629-766701,784067-784077
  Pixels:67664-67939,103093-103164,127982-128053,134113-134118,156609-156938,159094-159123,361338-361607,366551-366556,366618-366620,368145-368150,369790-369861,386826-386834
  PlaneInfo:294985-295076,362567-362590,382112-382135,386450,386451,548841-548950,552120-552129,1144955-1145044,1153609,1153610,1153927,1155387,1155388,1159625-1159648,1196771-1196773
  ChannelBinding:226182-226825,346195-346362,421761-421928,438535-438548,514129-514898,520709-520778,1131413-1132042,1142887-1142900,1142979-1142985,1149813-1149826,1154738-1154905,1225864-1225884
  QuantumDef:89860-90135,134272-134343,165124-165195,172202-172207,198959-199288,201493-201522,406680-406949,412540-412545,412574-412576,415093-415098,416903-416974,440306-440314
  RenderingDef:89860-90135,134272-134343,165124-165195,172202-172207,198959-199288,201493-201522,406680-406949,412540-412545,412574-412576,415093-415098,416903-416974,440299-440307
  Thumbnail:93999-94274,139686-139757,170750-170821,178247-178252,204922-205251,207615-207644,414764-415033,420626-420631,420694-420696,423433-423438,425337-425408,448341-448349
  Fileset:18622-18714,24709-24732,32231-32254,34362,34363,37352-37461,37888-37897,41661-41750,41911,41912,41922,42489,42490,42588-42611,45993-45995
  FilesetEntry:18622-18714,24714-24737,32285-32308,34416,34417,159802-159911,160338-160347,634011-634100,634261,634262,634272,634839,634840,634938-634961,638448-638450
  FilesetJobLink:91956-92420,121741-121860,159301-159420,169956-169965,184556-185105,187236-187285,205401-205850,206651-206660,206706-206710,209541-209550,210036-210155,226861-226875
  IndexingJob:107007,107012,107017,107022,107027,107032,107037,107042,107047,107052,107057,107062,107067,107072,107077,107082,107087,107092,107097,107102,107107,107112,107117,107122,107127,107132,107137,107142,107147,107152,107157,107162,107167,107172,107177,107182,107187,107192,107197,107202,107207,107212,107217,107222,107227,107232,107237,107242,107247,107252,107257,107262,107267,107272,107277,107282,107287,107292,107297,107302,107307,107312,107317,107322,107327,107332,107337,107342,107347,107352,107357,107362,107367,107372,107377,107382,107387,107392,107397,107402,107407,107412,107417,107422,107427,107432,107437,107442,107447,107452,107457,107462,107467,140164,140169,140174,140179,140184,140189,140194,140199,140204,140209,140214,140219,140224,140229,140234,140239,140244,140249,140254,140259,140264,140269,140274,140279,178647,178652,178657,178662,178667,178672,178677,178682,178687,178692,178697,178702,178707,178712,178717,178722,178727,178732,178737,178742,178747,178752,178757,178762,189872,189877,206128,206133,206138,206143,206148,206153,206158,206163,206168,206173,206178,206183,206188,206193,206198,206203,206208,206213,206218,206223,206228,206233,206238,206243,206248,206253,206258,206263,206268,206273,206278,206283,206288,206293,206298,206303,206308,206313,206318,206323,206328,206333,206338,206343,206348,206353,206358,206363,206368,206373,206378,206383,206388,206393,206398,206403,206408,206413,206418,206423,206428,206433,206438,206443,206448,206453,206458,206463,206468,206473,206478,206483,206488,206493,206498,206503,206508,206513,206518,206523,206528,206533,206538,206543,206548,206553,206558,206563,206568,206573,206578,206583,206588,206593,206598,206603,206608,206613,206618,206623,206628,206633,206638,206643,206648,206653,206658,206663,206668,206673,208973,208978,208983,208988,208993,208998,209003,209008,209013,209018,232086,232091,232096,232101,232106,232111,232116,232121,232126,232131,232136,232141,232146,232151,232156,232161,232166,232171,232176,232181,232186,232191,232196,232201,232206,232211,232216,232221,232226,232231,232236,232241,232246,232251,232256,232261,232266,232271,232276,232281,232286,232291,232296,232301,232306,232311,232316,232321,232326,232331,232336,232341,232346,232351,232356,232361,232366,232371,232376,232381,232386,232391,232396,232401,232406,232411,232416,232421,232426,232431,232436,232441,232446,232451,232456,232461,232466,232471,232476,232481,232486,232491,232496,232501,232506,232511,232516,232521,232526,232531,233957,233962,234040,237197,237202,238136,238141,238146,238151,238156,238161,238166,238171,238176,238181,238186,238191,238196,238201,238206,238211,238216,238221,238226,238231,238236,238241,238246,238251,261329,261334,261339
  JobOriginalFileLink:47951-48043,60194-60217,69547-69570,72670,72671,78735-78844,79601-79610,81643,81708,81710,92811-92900,94303,94304,94365,95553,95554,96464-96487,111316-111318
  MetadataImportJob:107004,107009,107014,107019,107024,107029,107034,107039,107044,107049,107054,107059,107064,107069,107074,107079,107084,107089,107094,107099,107104,107109,107114,107119,107124,107129,107134,107139,107144,107149,107154,107159,107164,107169,107174,107179,107184,107189,107194,107199,107204,107209,107214,107219,107224,107229,107234,107239,107244,107249,107254,107259,107264,107269,107274,107279,107284,107289,107294,107299,107304,107309,107314,107319,107324,107329,107334,107339,107344,107349,107354,107359,107364,107369,107374,107379,107384,107389,107394,107399,107404,107409,107414,107419,107424,107429,107434,107439,107444,107449,107454,107459,107464,140161,140166,140171,140176,140181,140186,140191,140196,140201,140206,140211,140216,140221,140226,140231,140236,140241,140246,140251,140256,140261,140266,140271,140276,178644,178649,178654,178659,178664,178669,178674,178679,178684,178689,178694,178699,178704,178709,178714,178719,178724,178729,178734,178739,178744,178749,178754,178759,189869,189874,206125,206130,206135,206140,206145,206150,206155,206160,206165,206170,206175,206180,206185,206190,206195,206200,206205,206210,206215,206220,206225,206230,206235,206240,206245,206250,206255,206260,206265,206270,206275,206280,206285,206290,206295,206300,206305,206310,206315,206320,206325,206330,206335,206340,206345,206350,206355,206360,206365,206370,206375,206380,206385,206390,206395,206400,206405,206410,206415,206420,206425,206430,206435,206440,206445,206450,206455,206460,206465,206470,206475,206480,206485,206490,206495,206500,206505,206510,206515,206520,206525,206530,206535,206540,206545,206550,206555,206560,206565,206570,206575,206580,206585,206590,206595,206600,206605,206610,206615,206620,206625,206630,206635,206640,206645,206650,206655,206660,206665,206670,208970,208975,208980,208985,208990,208995,209000,209005,209010,209015,232083,232088,232093,232098,232103,232108,232113,232118,232123,232128,232133,232138,232143,232148,232153,232158,232163,232168,232173,232178,232183,232188,232193,232198,232203,232208,232213,232218,232223,232228,232233,232238,232243,232248,232253,232258,232263,232268,232273,232278,232283,232288,232293,232298,232303,232308,232313,232318,232323,232328,232333,232338,232343,232348,232353,232358,232363,232368,232373,232378,232383,232388,232393,232398,232403,232408,232413,232418,232423,232428,232433,232438,232443,232448,232453,232458,232463,232468,232473,232478,232483,232488,232493,232498,232503,232508,232513,232518,232523,232528,233954,233959,234037,237194,237199,238133,238138,238143,238148,238153,238158,238163,238168,238173,238178,238183,238188,238193,238198,238203,238208,238213,238218,238223,238228,238233,238238,238243,238248,261326,261331,261336
  PixelDataJob:107005,107010,107015,107020,107025,107030,107035,107040,107045,107050,107055,107060,107065,107070,107075,107080,107085,107090,107095,107100,107105,107110,107115,107120,107125,107130,107135,107140,107145,107150,107155,107160,107165,107170,107175,107180,107185,107190,107195,107200,107205,107210,107215,107220,107225,107230,107235,107240,107245,107250,107255,107260,107265,107270,107275,107280,107285,107290,107295,107300,107305,107310,107315,107320,107325,107330,107335,107340,107345,107350,107355,107360,107365,107370,107375,107380,107385,107390,107395,107400,107405,107410,107415,107420,107425,107430,107435,107440,107445,107450,107455,107460,107465,140162,140167,140172,140177,140182,140187,140192,140197,140202,140207,140212,140217,140222,140227,140232,140237,140242,140247,140252,140257,140262,140267,140272,140277,178645,178650,178655,178660,178665,178670,178675,178680,178685,178690,178695,178700,178705,178710,178715,178720,178725,178730,178735,178740,178745,178750,178755,178760,189870,189875,206126,206131,206136,206141,206146,206151,206156,206161,206166,206171,206176,206181,206186,206191,206196,206201,206206,206211,206216,206221,206226,206231,206236,206241,206246,206251,206256,206261,206266,206271,206276,206281,206286,206291,206296,206301,206306,206311,206316,206321,206326,206331,206336,206341,206346,206351,206356,206361,206366,206371,206376,206381,206386,206391,206396,206401,206406,206411,206416,206421,206426,206431,206436,206441,206446,206451,206456,206461,206466,206471,206476,206481,206486,206491,206496,206501,206506,206511,206516,206521,206526,206531,206536,206541,206546,206551,206556,206561,206566,206571,206576,206581,206586,206591,206596,206601,206606,206611,206616,206621,206626,206631,206636,206641,206646,206651,206656,206661,206666,206671,208971,208976,208981,208986,208991,208996,209001,209006,209011,209016,232084,232089,232094,232099,232104,232109,232114,232119,232124,232129,232134,232139,232144,232149,232154,232159,232164,232169,232174,232179,232184,232189,232194,232199,232204,232209,232214,232219,232224,232229,232234,232239,232244,232249,232254,232259,232264,232269,232274,232279,232284,232289,232294,232299,232304,232309,232314,232319,232324,232329,232334,232339,232344,232349,232354,232359,232364,232369,232374,232379,232384,232389,232394,232399,232404,232409,232414,232419,232424,232429,232434,232439,232444,232449,232454,232459,232464,232469,232474,232479,232484,232489,232494,232499,232504,232509,232514,232519,232524,232529,233955,233960,234038,237195,237200,238134,238139,238144,238149,238154,238159,238164,238169,238174,238179,238184,238189,238194,238199,238204,238209,238214,238219,238224,238229,238234,238239,238244,238249,261327,261332,261337
  ScriptJob:210744,210854,210856
  ThumbnailGenerationJob:107006,107011,107016,107021,107026,107031,107036,107041,107046,107051,107056,107061,107066,107071,107076,107081,107086,107091,107096,107101,107106,107111,107116,107121,107126,107131,107136,107141,107146,107151,107156,107161,107166,107171,107176,107181,107186,107191,107196,107201,107206,107211,107216,107221,107226,107231,107236,107241,107246,107251,107256,107261,107266,107271,107276,107281,107286,107291,107296,107301,107306,107311,107316,107321,107326,107331,107336,107341,107346,107351,107356,107361,107366,107371,107376,107381,107386,107391,107396,107401,107406,107411,107416,107421,107426,107431,107436,107441,107446,107451,107456,107461,107466,140163,140168,140173,140178,140183,140188,140193,140198,140203,140208,140213,140218,140223,140228,140233,140238,140243,140248,140253,140258,140263,140268,140273,140278,178646,178651,178656,178661,178666,178671,178676,178681,178686,178691,178696,178701,178706,178711,178716,178721,178726,178731,178736,178741,178746,178751,178756,178761,189871,189876,206127,206132,206137,206142,206147,206152,206157,206162,206167,206172,206177,206182,206187,206192,206197,206202,206207,206212,206217,206222,206227,206232,206237,206242,206247,206252,206257,206262,206267,206272,206277,206282,206287,206292,206297,206302,206307,206312,206317,206322,206327,206332,206337,206342,206347,206352,206357,206362,206367,206372,206377,206382,206387,206392,206397,206402,206407,206412,206417,206422,206427,206432,206437,206442,206447,206452,206457,206462,206467,206472,206477,206482,206487,206492,206497,206502,206507,206512,206517,206522,206527,206532,206537,206542,206547,206552,206557,206562,206567,206572,206577,206582,206587,206592,206597,206602,206607,206612,206617,206622,206627,206632,206637,206642,206647,206652,206657,206662,206667,206672,208972,208977,208982,208987,208992,208997,209002,209007,209012,209017,232085,232090,232095,232100,232105,232110,232115,232120,232125,232130,232135,232140,232145,232150,232155,232160,232165,232170,232175,232180,232185,232190,232195,232200,232205,232210,232215,232220,232225,232230,232235,232240,232245,232250,232255,232260,232265,232270,232275,232280,232285,232290,232295,232300,232305,232310,232315,232320,232325,232330,232335,232340,232345,232350,232355,232360,232365,232370,232375,232380,232385,232390,232395,232400,232405,232410,232415,232420,232425,232430,232435,232440,232445,232450,232455,232460,232465,232470,232475,232480,232485,232490,232495,232500,232505,232510,232515,232520,232525,232530,233956,233961,234039,237196,237201,238135,238140,238145,238150,238155,238160,238165,238170,238175,238180,238185,238190,238195,238200,238205,238210,238215,238220,238225,238230,238235,238240,238245,238250,261328,261333,261338
  UploadJob:107003,107008,107013,107018,107023,107028,107033,107038,107043,107048,107053,107058,107063,107068,107073,107078,107083,107088,107093,107098,107103,107108,107113,107118,107123,107128,107133,107138,107143,107148,107153,107158,107163,107168,107173,107178,107183,107188,107193,107198,107203,107208,107213,107218,107223,107228,107233,107238,107243,107248,107253,107258,107263,107268,107273,107278,107283,107288,107293,107298,107303,107308,107313,107318,107323,107328,107333,107338,107343,107348,107353,107358,107363,107368,107373,107378,107383,107388,107393,107398,107403,107408,107413,107418,107423,107428,107433,107438,107443,107448,107453,107458,107463,140160,140165,140170,140175,140180,140185,140190,140195,140200,140205,140210,140215,140220,140225,140230,140235,140240,140245,140250,140255,140260,140265,140270,140275,178643,178648,178653,178658,178663,178668,178673,178678,178683,178688,178693,178698,178703,178708,178713,178718,178723,178728,178733,178738,178743,178748,178753,178758,189868,189873,206124,206129,206134,206139,206144,206149,206154,206159,206164,206169,206174,206179,206184,206189,206194,206199,206204,206209,206214,206219,206224,206229,206234,206239,206244,206249,206254,206259,206264,206269,206274,206279,206284,206289,206294,206299,206304,206309,206314,206319,206324,206329,206334,206339,206344,206349,206354,206359,206364,206369,206374,206379,206384,206389,206394,206399,206404,206409,206414,206419,206424,206429,206434,206439,206444,206449,206454,206459,206464,206469,206474,206479,206484,206489,206494,206499,206504,206509,206514,206519,206524,206529,206534,206539,206544,206549,206554,206559,206564,206569,206574,206579,206584,206589,206594,206599,206604,206609,206614,206619,206624,206629,206634,206639,206644,206649,206654,206659,206664,206669,208969,208974,208979,208984,208989,208994,208999,209004,209009,209014,232082,232087,232092,232097,232102,232107,232112,232117,232122,232127,232132,232137,232142,232147,232152,232157,232162,232167,232172,232177,232182,232187,232192,232197,232202,232207,232212,232217,232222,232227,232232,232237,232242,232247,232252,232257,232262,232267,232272,232277,232282,232287,232292,232297,232302,232307,232312,232317,232322,232327,232332,232337,232342,232347,232352,232357,232362,232367,232372,232377,232382,232387,232392,232397,232402,232407,232412,232417,232422,232427,232432,232437,232442,232447,232452,232457,232462,232467,232472,232477,232482,232487,232492,232497,232502,232507,232512,232517,232522,232527,233953,233958,234036,237193,237198,238132,238137,238142,238147,238152,238157,238162,238167,238172,238177,238182,238187,238192,238197,238202,238207,238212,238217,238222,238227,238232,238237,238242,238247,261325,261330,261335
  StatsInfo:129814-130181,210123-210218,250399-250494,260135-260142,314901-315340,319773-319812,919589-919948,928750-928757,928926-928929,932012-932019,936238-936333,985133-985144
```