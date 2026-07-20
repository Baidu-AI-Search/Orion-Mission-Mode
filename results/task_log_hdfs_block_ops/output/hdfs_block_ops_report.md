# HDFS DataNode Block Operations Report

**Source log:** `hdfs_datanode.log`  
**Total log lines parsed:** 2000  
**Total unique block IDs observed:** **390**

## 1. Block Inventory

The log contains **390 unique block IDs**:

- `blk_-1608999687919862906`
- `blk_7503483334202473044`
- `blk_-3544583377289625738`
- `blk_-9073992586687739851`
- `blk_7854771516489510256`
- `blk_1717858812220360316`
- `blk_-2519617320378473615`
- `blk_7063315473424667801`
- `blk_8586544123689943463`
- `blk_2765344736980045501`
- `blk_-2900490557492272760`
- `blk_-50273257731426871`
- `blk_4394112519745907149`
- `blk_3640100967125688321`
- `blk_-40115644493265216`
- `blk_-8531310335568756456`
- `blk_-3409923645141256069`
- `blk_3974948352784823938`
- `blk_5647760196018207394`
- `blk_-202775138379690649`
- `blk_-8120118743897862909`
- `blk_8376667364205250596`
- `blk_-1942808544656255720`
- `blk_3947106522258141922`
- `blk_-4513615671112005170`
- `blk_5703440264997337028`
- `blk_-66330728533676520`
- `blk_1998424538687454859`
- `blk_3175731177970777720`
- `blk_5117857107611435103`
- `blk_5727159100405160712`
- `blk_-6535155942125829332`
- `blk_-7742258871275669707`
- `blk_-7861579849076010389`
- `blk_-8476126489496204177`
- `blk_-2372508938534145884`
- `blk_4649698331936539655`
- `blk_9069966081657556515`
- `blk_-2444160001954743483`
- `blk_-2828839543885026602`
- `blk_-3940941529251864773`
- `blk_8609819439677503690`
- `blk_-8281945757273050140`
- `blk_2825871331379974767`
- `blk_-774246298521956028`
- `blk_-5649479540791129974`
- `blk_-8013855621109800549`
- `blk_177394776382448614`
- `blk_-2398209593415798905`
- `blk_-6370470857048627387`
- `blk_7475438553401908725`
- `blk_-8186438745381579117`
- `blk_-105450231192318816`
- `blk_-1385756122847916710`
- `blk_2080109799967615857`
- `blk_3098154771341398925`
- `blk_3738211383445750914`
- `blk_-4309280423174037990`
- `blk_584104991019184413`
- `blk_6609646171038203219`
- `blk_6740695140894845846`
- `blk_6835995323369082616`
- `blk_-7198899606504196854`
- `blk_7955481321933797799`
- `blk_7956543127401791181`
- `blk_8026466674001363449`
- `blk_-2850818623433938591`
- `blk_6227055072516687391`
- `blk_1937926427363440853`
- `blk_-2682415100025107597`
- `blk_-2814588473145762869`
- `blk_634338240549205708`
- `blk_7474637556625667220`
- `blk_-7911463493785581853`
- `blk_967160175355936210`
- `blk_3941533870808561940`
- `blk_40441923226500563`
- `blk_6309418393114897475`
- `blk_-6790654856691185731`
- `blk_8558795046002911094`
- `blk_-3667585438930153850`
- `blk_5479015932651254774`
- `blk_6918081507126070602`
- `blk_8046151943293893685`
- `blk_3461505966191484945`
- `blk_-4341181125838883663`
- `blk_5775293647706867248`
- `blk_6288745377407023095`
- `blk_-3076705858972714475`
- `blk_4521164666406024155`
- `blk_-5835224384325268050`
- `blk_-3102267849859399193`
- `blk_-3776330106018304557`
- `blk_-3865158146925189370`
- `blk_-6108780682959356968`
- `blk_6597014667441085597`
- `blk_-9025459096044337508`
- `blk_-1477913034248457629`
- `blk_1613550655988246573`
- `blk_2074627648226805983`
- `blk_2986822300704311308`
- `blk_-4390409968877773039`
- `blk_-8515113585876695879`
- `blk_8595954612153362607`
- `blk_8665604379324651807`
- `blk_-1187723472581877455`
- `blk_-1548346834251221499`
- `blk_-1962009378227535387`
- `blk_-2804479706331551014`
- `blk_4845477247445346914`
- `blk_-5702477348781461354`
- `blk_-5800700353547170271`
- `blk_-6920587680575966964`
- `blk_-7158210625015641227`
- `blk_-7226303435190136642`
- `blk_-8756305378882356508`
- `blk_8910234103556704078`
- `blk_2329219899967276279`
- `blk_-2741438423499484166`
- `blk_-4710160024743429585`
- `blk_5491460290951785997`
- `blk_-2983481749155087856`
- `blk_5161501523120226002`
- `blk_1325332921567711382`
- `blk_-1871778753987286456`
- `blk_-518459762181346762`
- `blk_2391384234620089834`
- `blk_3433546525085724680`
- `blk_-1076549517733373559`
- `blk_7427307448707327249`
- `blk_2912732363095595893`
- `blk_-4190491243436026170`
- `blk_-4667470578821022066`
- `blk_-8054782785917830420`
- `blk_-8091841062813936618`
- `blk_9210346052555304090`
- `blk_202536106610432362`
- `blk_-2869299916035182755`
- `blk_-4430564173662960642`
- `blk_-6586753944835843334`
- `blk_-6879190567969332918`
- `blk_-6931080266317198977`
- `blk_7352609810175565501`
- `blk_-859705208416062320`
- `blk_1404945464756167309`
- `blk_1448813076571847187`
- `blk_1912523661436659100`
- `blk_2347008916727134499`
- `blk_3508011185486088738`
- `blk_3880327230518840131`
- `blk_3972287319691001044`
- `blk_-4770972463483766707`
- `blk_-6125125239494402092`
- `blk_8059760933250408909`
- `blk_966870748231287658`
- `blk_1279101103527882599`
- `blk_-2598897828182146855`
- `blk_3341600009111698611`
- `blk_-4002888391906787542`
- `blk_4386079411548040260`
- `blk_4675103097330094461`
- `blk_5488900529276086615`
- `blk_-7518315834509097389`
- `blk_855704723224540977`
- `blk_1640563687655694592`
- `blk_-7851573941192785283`
- `blk_-2947515393396507971`
- `blk_3522654861930890662`
- `blk_-4673142718163027981`
- `blk_-7660193828821971101`
- `blk_-9166492953238830125`
- `blk_2689014683808259029`
- `blk_-2973260837493784784`
- `blk_3909865472571090536`
- `blk_-859147788836187186`
- `blk_-4393655256228529026`
- `blk_-5114249203202400596`
- `blk_-546318595782592836`
- `blk_3363069898317139269`
- `blk_8573973636575611103`
- `blk_-3758406899752824535`
- `blk_38865049064139660`
- `blk_-4229931861869531048`
- `blk_-1626340715627889482`
- `blk_2221775105544933826`
- `blk_4737741713837408345`
- `blk_-5950249832413179417`
- `blk_-8420426811026669438`
- `blk_-1470784088028862059`
- `blk_2366822833609442341`
- `blk_-2917825689581470793`
- `blk_-4046605047697127122`
- `blk_-5584251724032983856`
- `blk_6021477756386488418`
- `blk_7268266957111394674`
- `blk_8814359467727556626`
- `blk_-1700929423419481113`
- `blk_-2622300918011411840`
- `blk_3152503487390436165`
- `blk_-4365681458226063681`
- `blk_-5912999755522282446`
- `blk_-7559008592818043090`
- `blk_-8588230903310885315`
- `blk_9125494407446525156`
- `blk_2619290546489140930`
- `blk_-3283849791137044034`
- `blk_-7686748181966193443`
- `blk_-1916058035352472789`
- `blk_-2278104779962392523`
- `blk_-3193540685585849952`
- `blk_4931832500563355889`
- `blk_5133892961859808126`
- `blk_-1052739769153545987`
- `blk_4151093570962084251`
- `blk_6717969265771639561`
- `blk_-4236464647074379160`
- `blk_-2498004188306609167`
- `blk_5636792039332085446`
- `blk_-7185891569842971867`
- `blk_2583366788302307794`
- `blk_-4026330115303607086`
- `blk_-1577830978049349432`
- `blk_6420476111425645508`
- `blk_377236923047456543`
- `blk_8223024669447846632`
- `blk_4058804987355354315`
- `blk_4856031730010032819`
- `blk_-6339417867119146108`
- `blk_-6714222090039882252`
- `blk_4031055865781150544`
- `blk_8725561728667995755`
- `blk_-9084956447070300510`
- `blk_-8048421706779991679`
- `blk_-4525470997464616220`
- `blk_872694497849122755`
- `blk_-5170072115129389871`
- `blk_-5493359978973542887`
- `blk_-8162512552777886199`
- `blk_1185079144408607775`
- `blk_-6852866038059123001`
- `blk_6578809109018330119`
- `blk_-7130787197300034964`
- `blk_2657254091763574664`
- `blk_3091099087150179177`
- `blk_4623571410782847630`
- `blk_-568941302732430172`
- `blk_2404342940449629715`
- `blk_-5414183895914433338`
- `blk_613065710451569222`
- `blk_-67977319704233769`
- `blk_-8694359045337946818`
- `blk_24847521761041757`
- `blk_-3238412011236244640`
- `blk_-1121030458003600251`
- `blk_-6827329558602881823`
- `blk_7132759917805418067`
- `blk_7392010738495470190`
- `blk_8055148678985254863`
- `blk_-3520428728308625709`
- `blk_6986538622634403948`
- `blk_-6035208985581955117`
- `blk_6678472565712118087`
- `blk_7573381576932599374`
- `blk_197214010584922120`
- `blk_-3253829104116368139`
- `blk_-3443869707407490925`
- `blk_-7689031213313245416`
- `blk_3773669678166680940`
- `blk_841265344082922041`
- `blk_3488190436389958215`
- `blk_-5358363415355511925`
- `blk_5274692881287326658`
- `blk_-2573988170548478524`
- `blk_-5175722170941249815`
- `blk_2646137657036117634`
- `blk_-6232712486646639079`
- `blk_-7232800529359257484`
- `blk_2585807940696131461`
- `blk_-7486457612086258197`
- `blk_1732876454483854279`
- `blk_3294295097158489623`
- `blk_-6511728007995801856`
- `blk_3953559589275347835`
- `blk_-463158400553007438`
- `blk_5614249702379360530`
- `blk_38171424126089923`
- `blk_-7532913187987519950`
- `blk_-3318642146574997320`
- `blk_-6054385392244792487`
- `blk_-7545473931084004681`
- `blk_-237744713088101845`
- `blk_541458502420960920`
- `blk_-7565306891743376805`
- `blk_8465769948419925967`
- `blk_1367870633219053668`
- `blk_-3572954475094555028`
- `blk_2455917203220074754`
- `blk_-5472366049560759167`
- `blk_-2449017855856764848`
- `blk_-906929634502249537`
- `blk_981612145312864885`
- `blk_1383479986546835007`
- `blk_1585681987211758961`
- `blk_5678512043430077200`
- `blk_3093569883558689157`
- `blk_4083337788449963064`
- `blk_-4916781897012753844`
- `blk_7251107390153250071`
- `blk_7916151846227308238`
- `blk_-5671895892153119162`
- `blk_-669219608853085168`
- `blk_2044409903741578562`
- `blk_2327492205667109903`
- `blk_4036267293724823480`
- `blk_6055889596475060317`
- `blk_3878871310819862093`
- `blk_-2667064017324592326`
- `blk_8518644188978882261`
- `blk_8155607960458082281`
- `blk_2163147564840591022`
- `blk_6525645224298470536`
- `blk_4289625875479909480`
- `blk_3239380279521454817`
- `blk_-2870732575704612495`
- `blk_6121212718624278543`
- `blk_-3407920531979306897`
- `blk_-6795505241391858229`
- `blk_7182298358730791197`
- `blk_-5584875496006139565`
- `blk_-2864044183052157382`
- `blk_3043753876829603164`
- `blk_-1268704127005579599`
- `blk_2931242832797339515`
- `blk_-3901171193985872487`
- `blk_4082071052002133707`
- `blk_-3534835244149147539`
- `blk_-3616976684127351139`
- `blk_4712938983108943253`
- `blk_-8144145466118512432`
- `blk_8294878850903324975`
- `blk_-1442035270681298125`
- `blk_-4013352108974757296`
- `blk_8624561841460699343`
- `blk_4139091197886383131`
- `blk_8135292087230413975`
- `blk_4566277459864535342`
- `blk_-7713388298429426884`
- `blk_-3046627542547536054`
- `blk_5661848627083847913`
- `blk_-7194027365856463559`
- `blk_-6704225939638932097`
- `blk_1239909128373552502`
- `blk_-8741334127422264486`
- `blk_7804208990899438071`
- `blk_-5143286617671754617`
- `blk_8183815269486695335`
- `blk_6192538903136651854`
- `blk_7340860866556817481`
- `blk_1780513736067213693`
- `blk_6402803854715743550`
- `blk_8257277173936543986`
- `blk_2602936878633669240`
- `blk_3647098096660331647`
- `blk_-4147447158944475729`
- `blk_6733725894073150811`
- `blk_6745860875456203032`
- `blk_-6441832264813778096`
- `blk_-9032182111118378964`
- `blk_-994288696663095595`
- `blk_5579327064488516122`
- `blk_-2064366892727746021`
- `blk_7359269325129318656`
- `blk_8554832644956889891`
- `blk_328595803100712652`
- `blk_6005203045226256208`
- `blk_6687412102242126380`
- `blk_5167166035808601451`
- `blk_268467721592087811`
- `blk_1472526959254300198`
- `blk_8643402937543183462`
- `blk_7031936135774406790`
- `blk_-4255221183685916173`
- `blk_-7837133872976053297`
- `blk_-8991056407448381439`
- `blk_-2126554733521224025`
- `blk_1191349968162171906`
- `blk_1568365807861288923`
- `blk_1242317965473535156`
- `blk_4989545155659808163`
- `blk_7146075618207518599`

## 2. Operation Type Counts

| Operation Type | Occurrences |
|---|---|
| allocateBlock | 385 |
| Receiving | 1149 |
| Received | 19 |
| addStoredBlock | 19 |
| replicate | 4 |
| PacketResponder | 12 |

## 3. Block Lifecycle Tracking (allocate → receive → stored)

Blocks with a **complete** lifecycle observed in this log slice: **4**

### `blk_-1608999687919862906`

- **Allocated path:** `/mnt/hadoop/mapred/system/job_200811092030_0001/job.jar` (size 91178 bytes)
- **allocateBlock:** NameNode allocated the block for the file above.
- **Receiving (DataXceiver):** 10 event(s); initial writer source(s) 10.250.10.6, 10.250.14.224, 10.250.19.102, 10.251.107.19, 10.251.215.16, 10.251.31.5, 10.251.74.79; receiving datanodes 10.250.10.6, 10.250.14.224, 10.250.19.102, 10.251.107.19, 10.251.215.16, 10.251.31.5, 10.251.74.79
- **Received (acknowledged):** PacketResponder confirmed 3 pipeline replica(s) from 10.250.10.6, 10.250.14.224, 10.250.19.102; DataXceiver received complete on 10.250.14.224, 10.251.107.19, 10.251.215.16, 10.251.31.5, 10.251.74.79
- **PacketResponder terminations:** 3 pipeline responder thread(s) finished
- **addStoredBlock:** NameNode added 10 storage entry/entries: 10.250.10.6, 10.251.111.209, 10.250.14.224, 10.251.71.193, 10.251.215.16, 10.251.74.79, 10.251.107.19, 10.251.31.5, 10.251.71.240, 10.251.90.64

### `blk_7503483334202473044`

- **Allocated path:** `/mnt/hadoop/mapred/system/job_200811092030_0001/job.split` (size 233217 bytes)
- **allocateBlock:** NameNode allocated the block for the file above.
- **Receiving (DataXceiver):** 3 event(s); initial writer source(s) 10.250.19.102, 10.251.215.16, 10.251.71.16; receiving datanodes 10.250.19.102, 10.251.215.16, 10.251.71.16
- **Received (acknowledged):** PacketResponder confirmed 3 pipeline replica(s) from 10.250.19.102, 10.251.215.16, 10.251.71.16; DataXceiver received complete on n/a
- **PacketResponder terminations:** 3 pipeline responder thread(s) finished
- **addStoredBlock:** NameNode added 3 storage entry/entries: 10.251.106.10, 10.251.215.16, 10.251.71.16

### `blk_-3544583377289625738`

- **Allocated path:** `/mnt/hadoop/mapred/system/job_200811092030_0001/job.xml` (size 11971 bytes)
- **allocateBlock:** NameNode allocated the block for the file above.
- **Receiving (DataXceiver):** 3 event(s); initial writer source(s) 10.250.11.100, 10.250.19.102, 10.251.197.226; receiving datanodes 10.250.11.100, 10.250.19.102, 10.251.197.226
- **Received (acknowledged):** PacketResponder confirmed 3 pipeline replica(s) from 10.250.11.100, 10.250.19.102, 10.251.197.226; DataXceiver received complete on n/a
- **PacketResponder terminations:** 3 pipeline responder thread(s) finished
- **addStoredBlock:** NameNode added 3 storage entry/entries: 10.251.39.179, 10.250.11.100, 10.251.197.226

### `blk_-9073992586687739851`

- **Allocated path:** `/user/root/rand/_logs/history/ip-10-250-19-102.ec2.internal_1226291400491_job_200811092030_0001_conf.xml` (size 11977 bytes)
- **allocateBlock:** NameNode allocated the block for the file above.
- **Receiving (DataXceiver):** 3 event(s); initial writer source(s) 10.250.14.196, 10.250.19.102, 10.250.7.244; receiving datanodes 10.250.14.196, 10.250.19.102, 10.250.7.244
- **Received (acknowledged):** PacketResponder confirmed 3 pipeline replica(s) from 10.250.14.196, 10.250.19.102, 10.250.7.244; DataXceiver received complete on n/a
- **PacketResponder terminations:** 3 pipeline responder thread(s) finished
- **addStoredBlock:** NameNode added 3 storage entry/entries: 10.251.89.155, 10.250.7.244, 10.250.14.196

### Blocks with partial/incomplete lifecycle (truncated by log window)

- `blk_7854771516489510256` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000079_0/part-00079`
- `blk_1717858812220360316` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000111_0/part-00111`
- `blk_-2519617320378473615` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000121_0/part-00121`
- `blk_7063315473424667801` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000122_0/part-00122`
- `blk_8586544123689943463` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000015_0/part-00015`
- `blk_2765344736980045501` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000086_0/part-00086`
- `blk_-2900490557492272760` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000098_0/part-00098`
- `blk_-50273257731426871` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000084_0/part-00084`
- `blk_4394112519745907149` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000081_0/part-00081`
- `blk_3640100967125688321` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000010_0/part-00010`
- `blk_-40115644493265216` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000042_0/part-00042`
- `blk_-8531310335568756456` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000007_0/part-00007`
- `blk_-3409923645141256069` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000002_0/part-00002`
- `blk_3974948352784823938` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000112_0/part-00112`
- `blk_5647760196018207394` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000131_0/part-00131`
- `blk_-202775138379690649` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000141_0/part-00141`
- `blk_-8120118743897862909` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000117_0/part-00117`
- `blk_8376667364205250596` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000123_0/part-00123`
- `blk_-1942808544656255720` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000120_0/part-00120`
- `blk_3947106522258141922` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000005_0/part-00005`
- `blk_-4513615671112005170` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000011_0/part-00011`
- `blk_5703440264997337028` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000124_0/part-00124`
- `blk_-66330728533676520` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000119_0/part-00119`
- `blk_1998424538687454859` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000143_0/part-00143`
- `blk_3175731177970777720` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000058_0/part-00058`
- `blk_5117857107611435103` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000048_0/part-00048`
- `blk_5727159100405160712` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000026_0/part-00026`
- `blk_-6535155942125829332` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000009_0/part-00009`
- `blk_-7742258871275669707` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000126_0/part-00126`
- `blk_-7861579849076010389` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000034_0/part-00034`
- `blk_-8476126489496204177` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000024_0/part-00024`
- `blk_-2372508938534145884` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000118_0/part-00118`
- `blk_4649698331936539655` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000127_0/part-00127`
- `blk_9069966081657556515` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000031_0/part-00031`
- `blk_-2444160001954743483` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000022_0/part-00022`
- `blk_-2828839543885026602` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000021_0/part-00021`
- `blk_-3940941529251864773` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000132_0/part-00132`
- `blk_8609819439677503690` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000060_0/part-00060`
- `blk_-8281945757273050140` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000006_0/part-00006`
- `blk_2825871331379974767` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000045_0/part-00045`
- `blk_-774246298521956028` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000218_0/part-00218`
- `blk_-5649479540791129974` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000012_0/part-00012`
- `blk_-8013855621109800549` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000134_0/part-00134`
- `blk_177394776382448614` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000244_0/part-00244`
- `blk_-2398209593415798905` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000054_0/part-00054`
- `blk_-6370470857048627387` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000044_0/part-00044`
- `blk_7475438553401908725` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000130_0/part-00130`
- `blk_-8186438745381579117` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000136_0/part-00136`
- `blk_-105450231192318816` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000128_0/part-00128`
- `blk_-1385756122847916710` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000221_0/part-00221`
- `blk_2080109799967615857` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000216_0/part-00216`
- `blk_3098154771341398925` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000029_0/part-00029`
- `blk_3738211383445750914` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000050_0/part-00050`
- `blk_-4309280423174037990` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000139_0/part-00139`
- `blk_584104991019184413` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000030_0/part-00030`
- `blk_6609646171038203219` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000229_0/part-00229`
- `blk_6740695140894845846` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000137_0/part-00137`
- `blk_6835995323369082616` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000223_0/part-00223`
- `blk_-7198899606504196854` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000133_0/part-00133`
- `blk_7955481321933797799` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000037_0/part-00037`
- `blk_7956543127401791181` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000017_0/part-00017`
- `blk_8026466674001363449` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000040_0/part-00040`
- `blk_-2850818623433938591` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000038_0/part-00038`
- `blk_6227055072516687391` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000008_0/part-00008`
- `blk_1937926427363440853` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000032_0/part-00032`
- `blk_-2682415100025107597` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000138_0/part-00138`
- `blk_-2814588473145762869` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000135_0/part-00135`
- `blk_634338240549205708` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000114_0/part-00114`
- `blk_7474637556625667220` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000125_0/part-00125`
- `blk_-7911463493785581853` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000142_0/part-00142`
- `blk_967160175355936210` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000233_0/part-00233`
- `blk_3941533870808561940` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000129_0/part-00129`
- `blk_40441923226500563` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000140_0/part-00140`
- `blk_6309418393114897475` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000035_0/part-00035`
- `blk_-6790654856691185731` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000061_0/part-00061`
- `blk_8558795046002911094` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000144_0/part-00144`
- `blk_-3667585438930153850` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000014_0/part-00014`
- `blk_5479015932651254774` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000146_0/part-00146`
- `blk_6918081507126070602` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000056_0/part-00056`
- `blk_8046151943293893685` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000018_0/part-00018`
- `blk_3461505966191484945` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000027_0/part-00027`
- `blk_-4341181125838883663` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000219_0/part-00219`
- `blk_5775293647706867248` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000230_0/part-00230`
- `blk_6288745377407023095` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000280_0/part-00280`
- `blk_-3076705858972714475` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000265_0/part-00265`
- `blk_4521164666406024155` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000046_0/part-00046`
- `blk_-5835224384325268050` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000004_0/part-00004`
- `blk_-3102267849859399193` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000249_0/part-00249`
- `blk_-3776330106018304557` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000064_0/part-00064`
- `blk_-3865158146925189370` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000145_0/part-00145`
- `blk_-6108780682959356968` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000245_0/part-00245`
- `blk_6597014667441085597` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000019_0/part-00019`
- `blk_-9025459096044337508` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000231_0/part-00231`
- `blk_-1477913034248457629` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000063_0/part-00063`
- `blk_1613550655988246573` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000224_0/part-00224`
- `blk_2074627648226805983` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000020_0/part-00020`
- `blk_2986822300704311308` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000094_0/part-00094`
- `blk_-4390409968877773039` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000043_0/part-00043`
- `blk_-8515113585876695879` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000065_0/part-00065`
- `blk_8595954612153362607` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000016_0/part-00016`
- `blk_8665604379324651807` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000039_0/part-00039`
- `blk_-1187723472581877455` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000210_0/part-00210`
- `blk_-1548346834251221499` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000049_0/part-00049`
- `blk_-1962009378227535387` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000013_0/part-00013`
- `blk_-2804479706331551014` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000217_0/part-00217`
- `blk_4845477247445346914` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000059_0/part-00059`
- `blk_-5702477348781461354` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000262_0/part-00262`
- `blk_-5800700353547170271` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000025_0/part-00025`
- `blk_-6920587680575966964` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000248_0/part-00248`
- `blk_-7158210625015641227` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000033_0/part-00033`
- `blk_-7226303435190136642` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000001_0/part-00001`
- `blk_-8756305378882356508` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000225_0/part-00225`
- `blk_8910234103556704078` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000270_0/part-00270`
- `blk_2329219899967276279` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000000_0/part-00000`
- `blk_-2741438423499484166` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000232_0/part-00232`
- `blk_-4710160024743429585` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000222_0/part-00222`
- `blk_5491460290951785997` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000266_0/part-00266`
- `blk_-2983481749155087856` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000070_0/part-00070`
- `blk_5161501523120226002` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000211_0/part-00211`
- `blk_1325332921567711382` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000055_0/part-00055`
- `blk_-1871778753987286456` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000282_0/part-00282`
- `blk_-518459762181346762` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000228_0/part-00228`
- `blk_2391384234620089834` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000267_0/part-00267`
- `blk_3433546525085724680` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000247_0/part-00247`
- `blk_-1076549517733373559` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000069_0/part-00069`
- `blk_7427307448707327249` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000276_0/part-00276`
- `blk_2912732363095595893` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000272_0/part-00272`
- `blk_-4190491243436026170` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000053_0/part-00053`
- `blk_-4667470578821022066` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000082_0/part-00082`
- `blk_-8054782785917830420` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000281_0/part-00281`
- `blk_-8091841062813936618` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000041_0/part-00041`
- `blk_9210346052555304090` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000288_0/part-00288`
- `blk_202536106610432362` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000294_0/part-00294`
- `blk_-2869299916035182755` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000092_0/part-00092`
- `blk_-4430564173662960642` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000301_0/part-00301`
- `blk_-6586753944835843334` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000078_0/part-00078`
- `blk_-6879190567969332918` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000283_0/part-00283`
- `blk_-6931080266317198977` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000087_0/part-00087`
- `blk_7352609810175565501` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000277_0/part-00277`
- `blk_-859705208416062320` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000205_0/part-00205`
- `blk_1404945464756167309` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000051_0/part-00051`
- `blk_1448813076571847187` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000089_0/part-00089`
- `blk_1912523661436659100` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000268_0/part-00268`
- `blk_2347008916727134499` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000101_0/part-00101`
- `blk_3508011185486088738` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000312_0/part-00312`
- `blk_3880327230518840131` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000295_0/part-00295`
- `blk_3972287319691001044` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000072_0/part-00072`
- `blk_-4770972463483766707` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000085_0/part-00085`
- `blk_-6125125239494402092` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000284_0/part-00284`
- `blk_8059760933250408909` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000167_0/part-00167`
- `blk_966870748231287658` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000093_0/part-00093`
- `blk_1279101103527882599` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000287_0/part-00287`
- `blk_-2598897828182146855` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000057_0/part-00057`
- `blk_3341600009111698611` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000083_0/part-00083`
- `blk_-4002888391906787542` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000090_0/part-00090`
- `blk_4386079411548040260` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000271_0/part-00271`
- `blk_4675103097330094461` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000256_0/part-00256`
- `blk_5488900529276086615` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000275_0/part-00275`
- `blk_-7518315834509097389` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000246_0/part-00246`
- `blk_855704723224540977` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000047_0/part-00047`
- `blk_1640563687655694592` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000103_0/part-00103`
- `blk_-7851573941192785283` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000074_0/part-00074`
- `blk_-2947515393396507971` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000286_0/part-00286`
- `blk_3522654861930890662` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000269_0/part-00269`
- `blk_-4673142718163027981` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000279_0/part-00279`
- `blk_-7660193828821971101` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000273_0/part-00273`
- `blk_-9166492953238830125` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000028_0/part-00028`
- `blk_2689014683808259029` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000261_0/part-00261`
- `blk_-2973260837493784784` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000299_0/part-00299`
- `blk_3909865472571090536` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000110_0/part-00110`
- `blk_-859147788836187186` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000036_0/part-00036`
- `blk_-4393655256228529026` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000304_0/part-00304`
- `blk_-5114249203202400596` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000075_0/part-00075`
- `blk_-546318595782592836` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000100_0/part-00100`
- `blk_3363069898317139269` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000274_0/part-00274`
- `blk_8573973636575611103` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000179_0/part-00179`
- `blk_-3758406899752824535` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000278_0/part-00278`
- `blk_38865049064139660` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000310_0/part-00310`
- `blk_-4229931861869531048` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000297_0/part-00297`
- `blk_-1626340715627889482` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000236_0/part-00236`
- `blk_2221775105544933826` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000067_0/part-00067`
- `blk_4737741713837408345` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000151_0/part-00151`
- `blk_-5950249832413179417` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000184_0/part-00184`
- `blk_-8420426811026669438` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000302_0/part-00302`
- `blk_-1470784088028862059` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000311_0/part-00311`
- `blk_2366822833609442341` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000102_0/part-00102`
- `blk_-2917825689581470793` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000315_0/part-00315`
- `blk_-4046605047697127122` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000066_0/part-00066`
- `blk_-5584251724032983856` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000115_0/part-00115`
- `blk_6021477756386488418` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000290_0/part-00290`
- `blk_7268266957111394674` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000003_0/part-00003`
- `blk_8814359467727556626` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000088_0/part-00088`
- `blk_-1700929423419481113` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000220_0/part-00220`
- `blk_-2622300918011411840` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000068_0/part-00068`
- `blk_3152503487390436165` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000105_0/part-00105`
- `blk_-4365681458226063681` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000300_0/part-00300`
- `blk_-5912999755522282446` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000308_0/part-00308`
- `blk_-7559008592818043090` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000106_0/part-00106`
- `blk_-8588230903310885315` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000116_0/part-00116`
- `blk_9125494407446525156` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000292_0/part-00292`
- `blk_2619290546489140930` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000320_0/part-00320`
- `blk_-3283849791137044034` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000298_0/part-00298`
- `blk_-7686748181966193443` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000293_0/part-00293`
- `blk_-1916058035352472789` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000173_0/part-00173`
- `blk_-2278104779962392523` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000023_0/part-00023`
- `blk_-3193540685585849952` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000107_0/part-00107`
- `blk_4931832500563355889` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000307_0/part-00307`
- `blk_5133892961859808126` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000314_0/part-00314`
- `blk_-1052739769153545987` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000073_0/part-00073`
- `blk_4151093570962084251` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000285_0/part-00285`
- `blk_6717969265771639561` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000097_0/part-00097`
- `blk_-4236464647074379160` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000071_0/part-00071`
- `blk_-2498004188306609167` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000076_0/part-00076`
- `blk_5636792039332085446` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000289_0/part-00289`
- `blk_-7185891569842971867` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000080_0/part-00080`
- `blk_2583366788302307794` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000154_0/part-00154`
- `blk_-4026330115303607086` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000332_0/part-00332`
- `blk_-1577830978049349432` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000077_0/part-00077`
- `blk_6420476111425645508` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000337_0/part-00337`
- `blk_377236923047456543` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000168_0/part-00168`
- `blk_8223024669447846632` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000305_0/part-00305`
- `blk_4058804987355354315` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000306_0/part-00306`
- `blk_4856031730010032819` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000259_0/part-00259`
- `blk_-6339417867119146108` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000317_0/part-00317`
- `blk_-6714222090039882252` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000149_0/part-00149`
- `blk_4031055865781150544` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000162_0/part-00162`
- `blk_8725561728667995755` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000318_0/part-00318`
- `blk_-9084956447070300510` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000062_0/part-00062`
- `blk_-8048421706779991679` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000099_0/part-00099`
- `blk_-4525470997464616220` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000303_0/part-00303`
- `blk_872694497849122755` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000191_0/part-00191`
- `blk_-5170072115129389871` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000313_0/part-00313`
- `blk_-5493359978973542887` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000291_0/part-00291`
- `blk_-8162512552777886199` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000331_0/part-00331`
- `blk_1185079144408607775` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000328_0/part-00328`
- `blk_-6852866038059123001` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000333_0/part-00333`
- `blk_6578809109018330119` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000096_0/part-00096`
- `blk_-7130787197300034964` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000329_0/part-00329`
- `blk_2657254091763574664` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000336_0/part-00336`
- `blk_3091099087150179177` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000175_0/part-00175`
- `blk_4623571410782847630` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000342_0/part-00342`
- `blk_-568941302732430172` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000147_0/part-00147`
- `blk_2404342940449629715` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000340_0/part-00340`
- `blk_-5414183895914433338` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000212_0/part-00212`
- `blk_613065710451569222` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000325_0/part-00325`
- `blk_-67977319704233769` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000150_0/part-00150`
- `blk_-8694359045337946818` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000052_0/part-00052`
- `blk_24847521761041757` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000156_0/part-00156`
- `blk_-3238412011236244640` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000161_0/part-00161`
- `blk_-1121030458003600251` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000335_0/part-00335`
- `blk_-6827329558602881823` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000321_0/part-00321`
- `blk_7132759917805418067` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000104_0/part-00104`
- `blk_7392010738495470190` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000316_0/part-00316`
- `blk_8055148678985254863` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000157_0/part-00157`
- `blk_-3520428728308625709` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000108_0/part-00108`
- `blk_6986538622634403948` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000165_0/part-00165`
- `blk_-6035208985581955117` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000334_0/part-00334`
- `blk_6678472565712118087` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000324_0/part-00324`
- `blk_7573381576932599374` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000190_0/part-00190`
- `blk_197214010584922120` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000186_0/part-00186`
- `blk_-3253829104116368139` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000326_0/part-00326`
- `blk_-3443869707407490925` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000319_0/part-00319`
- `blk_-7689031213313245416` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000327_0/part-00327`
- `blk_3773669678166680940` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000322_0/part-00322`
- `blk_841265344082922041` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000296_0/part-00296`
- `blk_3488190436389958215` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000309_0/part-00309`
- `blk_-5358363415355511925` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000209_0/part-00209`
- `blk_5274692881287326658` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000343_0/part-00343`
- `blk_-2573988170548478524` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000155_0/part-00155`
- `blk_-5175722170941249815` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000365_0/part-00365`
- `blk_2646137657036117634` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000338_0/part-00338`
- `blk_-6232712486646639079` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000345_0/part-00345`
- `blk_-7232800529359257484` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000109_0/part-00109`
- `blk_2585807940696131461` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000351_0/part-00351`
- `blk_-7486457612086258197` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000323_0/part-00323`
- `blk_1732876454483854279` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000169_0/part-00169`
- `blk_3294295097158489623` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000163_0/part-00163`
- `blk_-6511728007995801856` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000187_0/part-00187`
- `blk_3953559589275347835` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000185_0/part-00185`
- `blk_-463158400553007438` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000170_0/part-00170`
- `blk_5614249702379360530` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000341_0/part-00341`
- `blk_38171424126089923` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000330_0/part-00330`
- `blk_-7532913187987519950` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000171_0/part-00171`
- `blk_-3318642146574997320` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000177_0/part-00177`
- `blk_-6054385392244792487` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000353_0/part-00353`
- `blk_-7545473931084004681` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000193_0/part-00193`
- `blk_-237744713088101845` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000347_0/part-00347`
- `blk_541458502420960920` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000356_0/part-00356`
- `blk_-7565306891743376805` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000372_0/part-00372`
- `blk_8465769948419925967` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000377_0/part-00377`
- `blk_1367870633219053668` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000358_0/part-00358`
- `blk_-3572954475094555028` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000195_0/part-00195`
- `blk_2455917203220074754` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000371_0/part-00371`
- `blk_-5472366049560759167` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000366_0/part-00366`
- `blk_-2449017855856764848` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000361_0/part-00361`
- `blk_-906929634502249537` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000360_0/part-00360`
- `blk_981612145312864885` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000237_0/part-00237`
- `blk_1383479986546835007` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000158_0/part-00158`
- `blk_1585681987211758961` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000113_0/part-00113`
- `blk_5678512043430077200` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000166_0/part-00166`
- `blk_3093569883558689157` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000188_0/part-00188`
- `blk_4083337788449963064` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000348_0/part-00348`
- `blk_-4916781897012753844` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000213_0/part-00213`
- `blk_7251107390153250071` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000095_0/part-00095`
- `blk_7916151846227308238` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000208_0/part-00208`
- `blk_-5671895892153119162` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000350_0/part-00350`
- `blk_-669219608853085168` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000346_0/part-00346`
- `blk_2044409903741578562` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000363_0/part-00363`
- `blk_2327492205667109903` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000374_0/part-00374`
- `blk_4036267293724823480` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000370_0/part-00370`
- `blk_6055889596475060317` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000354_0/part-00354`
- `blk_3878871310819862093` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000364_0/part-00364`
- `blk_-2667064017324592326` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000148_0/part-00148`
- `blk_8518644188978882261` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000344_0/part-00344`
- `blk_8155607960458082281` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000189_0/part-00189`
- `blk_2163147564840591022` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000352_0/part-00352`
- `blk_6525645224298470536` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000192_0/part-00192`
- `blk_4289625875479909480` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000206_0/part-00206`
- `blk_3239380279521454817` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000176_0/part-00176`
- `blk_-2870732575704612495` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000204_0/part-00204`
- `blk_6121212718624278543` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000253_0/part-00253`
- `blk_-3407920531979306897` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000375_0/part-00375`
- `blk_-6795505241391858229` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000367_0/part-00367`
- `blk_7182298358730791197` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000369_0/part-00369`
- `blk_-5584875496006139565` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000091_0/part-00091`
- `blk_-2864044183052157382` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000368_0/part-00368`
- `blk_3043753876829603164` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000152_0/part-00152`
- `blk_-1268704127005579599` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000357_0/part-00357`
- `blk_2931242832797339515` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000183_0/part-00183`
- `blk_-3901171193985872487` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000384_0/part-00384`
- `blk_4082071052002133707` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000159_0/part-00159`
- `blk_-3534835244149147539` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000196_0/part-00196`
- `blk_-3616976684127351139` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000349_0/part-00349`
- `blk_4712938983108943253` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000359_0/part-00359`
- `blk_-8144145466118512432` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000355_0/part-00355`
- `blk_8294878850903324975` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000178_0/part-00178`
- `blk_-1442035270681298125` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000215_0/part-00215`
- `blk_-4013352108974757296` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000182_0/part-00182`
- `blk_8624561841460699343` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000180_0/part-00180`
- `blk_4139091197886383131` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000198_0/part-00198`
- `blk_8135292087230413975` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000378_0/part-00378`
- `blk_4566277459864535342` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000362_0/part-00362`
- `blk_-7713388298429426884` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000381_0/part-00381`
- `blk_-3046627542547536054` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000251_0/part-00251`
- `blk_5661848627083847913` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000257_0/part-00257`
- `blk_-7194027365856463559` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000197_0/part-00197`
- `blk_-6704225939638932097` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000383_0/part-00383`
- `blk_1239909128373552502` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000174_0/part-00174`
- `blk_-8741334127422264486` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000376_0/part-00376`
- `blk_7804208990899438071` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000258_0/part-00258`
- `blk_-5143286617671754617` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000172_0/part-00172`
- `blk_8183815269486695335` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000181_0/part-00181`
- `blk_6192538903136651854` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000238_0/part-00238`
- `blk_7340860866556817481` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000382_0/part-00382`
- `blk_1780513736067213693` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000194_0/part-00194`
- `blk_6402803854715743550` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000388_0/part-00388`
- `blk_8257277173936543986` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000385_0/part-00385`
- `blk_2602936878633669240` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000380_0/part-00380`
- `blk_3647098096660331647` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000373_0/part-00373`
- `blk_-4147447158944475729` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000207_0/part-00207`
- `blk_6733725894073150811` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000386_0/part-00386`
- `blk_6745860875456203032` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000201_0/part-00201`
- `blk_-6441832264813778096` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000379_0/part-00379`
- `blk_-9032182111118378964` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000202_0/part-00202`
- `blk_-994288696663095595` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000203_0/part-00203`
- `blk_5579327064488516122` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000235_0/part-00235`
- `blk_-2064366892727746021` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000242_0/part-00242`
- `blk_7359269325129318656` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000164_0/part-00164`
- `blk_8554832644956889891` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000387_0/part-00387`
- `blk_328595803100712652` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000339_0/part-00339`
- `blk_6005203045226256208` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000214_0/part-00214`
- `blk_6687412102242126380` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000389_0/part-00389`
- `blk_5167166035808601451` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000239_0/part-00239`
- `blk_268467721592087811` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000153_0/part-00153`
- `blk_1472526959254300198` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000392_0/part-00392`
- `blk_8643402937543183462` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000160_0/part-00160`
- `blk_7031936135774406790` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000254_0/part-00254`
- `blk_-4255221183685916173` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000264_0/part-00264`
- `blk_-7837133872976053297` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000260_0/part-00260`
- `blk_-8991056407448381439` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000394_0/part-00394`
- `blk_-2126554733521224025` — missing: Received, addStoredBlock; path: `/user/root/rand/_temporary/_task_200811092030_0001_m_000200_0/part-00200`
- `blk_1191349968162171906` — missing: allocateBlock, Received, addStoredBlock; path: `None`
- `blk_1568365807861288923` — missing: allocateBlock, Received, addStoredBlock; path: `None`
- `blk_1242317965473535156` — missing: allocateBlock, Received, addStoredBlock; path: `None`
- `blk_4989545155659808163` — missing: allocateBlock, Received, addStoredBlock; path: `None`
- `blk_7146075618207518599` — missing: allocateBlock, Received, addStoredBlock; path: `None`

## 4. Replication Chain

Blocks with explicit replication activity: **1**

### `blk_-1608999687919862906`

**NameNode replication commands:**
- Source datanode `10.250.14.224:50010` asked to replicate to → `10.251.215.16:50010`, `10.251.71.193:50010`
- Source datanode `10.251.215.16:50010` asked to replicate to → `10.251.74.79:50010`, `10.251.107.19:50010`
- Source datanode `10.251.107.19:50010` asked to replicate to → `10.251.31.5:50010`, `10.251.71.240:50010`
- Source datanode `10.251.31.5:50010` asked to replicate to → `10.251.90.64:50010`
**Transfer threads started on source DataNodes:**
- `10.250.14.224:50010` starting transfer thread to → `10.251.215.16:50010`, `10.251.71.193:50010`
- `10.251.215.16:50010` starting transfer thread to → `10.251.74.79:50010`, `10.251.107.19:50010`
- `10.251.107.19:50010` starting transfer thread to → `10.251.31.5:50010`, `10.251.71.240:50010`
- `10.251.31.5:50010` starting transfer thread to → `10.251.90.64:50010`
**Observed block transmissions (source → destination):**
- `10.250.14.224:50010` ➜ `10.251.215.16:50010`
- `10.251.107.19:50010` ➜ `10.251.31.5:50010`
- `10.251.215.16:50010` ➜ `10.251.74.79:50010`
- `10.251.31.5:50010` ➜ `10.251.90.64:50010`

## 5. Associated MapReduce Jobs

Unique MapReduce job IDs detected in HDFS paths: **1**

### `job_200811092030_0001`

- Path `/mnt/hadoop/mapred/system/job_200811092030_0001/job.jar` → blocks: `blk_-1608999687919862906`
- Path `/mnt/hadoop/mapred/system/job_200811092030_0001/job.split` → blocks: `blk_7503483334202473044`
- Path `/mnt/hadoop/mapred/system/job_200811092030_0001/job.xml` → blocks: `blk_-3544583377289625738`
- Path `/user/root/rand/_logs/history/ip-10-250-19-102.ec2.internal_1226291400491_job_200811092030_0001_conf.xml` → blocks: `blk_-9073992586687739851`

## 6. Per-Block Detail Table

| Block ID | Size (bytes) | Allocated Path | Nodes Involved (IPs) | Replication Count (addStoredBlock) |
|---|---:|---|---|---:|
| `blk_-1608999687919862906` | 91178 | `/mnt/hadoop/mapred/system/job_200811092030_0001/job.jar` | 10.250.10.6, 10.250.14.224, 10.250.19.102, 10.251.107.19, 10.251.111.209, 10.251.215.16, 10.251.31.5, 10.251.71.193, 10.251.71.240, 10.251.74.79, 10.251.90.64 | 10 |
| `blk_7503483334202473044` | 233217 | `/mnt/hadoop/mapred/system/job_200811092030_0001/job.split` | 10.250.19.102, 10.251.106.10, 10.251.215.16, 10.251.71.16 | 3 |
| `blk_-3544583377289625738` | 11971 | `/mnt/hadoop/mapred/system/job_200811092030_0001/job.xml` | 10.250.11.100, 10.250.19.102, 10.251.197.226, 10.251.39.179 | 3 |
| `blk_-9073992586687739851` | 11977 | `/user/root/rand/_logs/history/ip-10-250-19-102.ec2.internal_1226291400491_job_200811092030_0001_conf.xml` | 10.250.14.196, 10.250.19.102, 10.250.7.244, 10.251.89.155 | 3 |
| `blk_7854771516489510256` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000079_0/part-00079` | 10.251.215.16, 10.251.74.227 | 0 |
| `blk_1717858812220360316` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000111_0/part-00111` | 10.251.127.191, 10.251.26.8 | 0 |
| `blk_-2519617320378473615` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000121_0/part-00121` | 10.250.13.188, 10.251.126.5 | 0 |
| `blk_7063315473424667801` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000122_0/part-00122` | 10.251.126.227, 10.251.127.191 | 0 |
| `blk_8586544123689943463` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000015_0/part-00015` | 10.251.125.193, 10.251.42.84 | 0 |
| `blk_2765344736980045501` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000086_0/part-00086` | 10.251.203.149, 10.251.75.79 | 0 |
| `blk_-2900490557492272760` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000098_0/part-00098` | 10.250.14.143, 10.251.30.134 | 0 |
| `blk_-50273257731426871` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000084_0/part-00084` | 10.251.31.5, 10.251.39.64 | 0 |
| `blk_4394112519745907149` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000081_0/part-00081` | 10.250.18.114, 10.251.107.98 | 0 |
| `blk_3640100967125688321` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000010_0/part-00010` | 10.250.18.114, 10.251.71.240 | 0 |
| `blk_-40115644493265216` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000042_0/part-00042` | 10.250.17.225, 10.251.215.16 | 0 |
| `blk_-8531310335568756456` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000007_0/part-00007` | 10.251.106.10, 10.251.203.149 | 0 |
| `blk_-3409923645141256069` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000002_0/part-00002` | 10.251.31.5, 10.251.91.229 | 0 |
| `blk_3974948352784823938` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000112_0/part-00112` | 10.251.125.193, 10.251.91.32 | 0 |
| `blk_5647760196018207394` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000131_0/part-00131` | 10.251.127.243, 10.251.215.16 | 0 |
| `blk_-202775138379690649` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000141_0/part-00141` | 10.251.127.47, 10.251.42.9 | 0 |
| `blk_-8120118743897862909` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000117_0/part-00117` | 10.251.111.209, 10.251.127.243 | 0 |
| `blk_8376667364205250596` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000123_0/part-00123` | 10.250.17.225, 10.251.126.255 | 0 |
| `blk_-1942808544656255720` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000120_0/part-00120` | 10.251.30.85 | 0 |
| `blk_3947106522258141922` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000005_0/part-00005` | 10.251.30.85, 10.251.67.4 | 0 |
| `blk_-4513615671112005170` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000011_0/part-00011` | 10.251.30.134, 10.251.70.211 | 0 |
| `blk_5703440264997337028` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000124_0/part-00124` | 10.251.126.83, 10.251.66.63 | 0 |
| `blk_-66330728533676520` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000119_0/part-00119` | 10.251.39.160, 10.251.65.203 | 0 |
| `blk_1998424538687454859` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000143_0/part-00143` | 10.251.30.179, 10.251.31.160 | 0 |
| `blk_3175731177970777720` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000058_0/part-00058` | 10.251.111.209, 10.251.65.237 | 0 |
| `blk_5117857107611435103` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000048_0/part-00048` | 10.251.198.196, 10.251.66.192 | 0 |
| `blk_5727159100405160712` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000026_0/part-00026` | 10.251.66.63, 10.251.89.155 | 0 |
| `blk_-6535155942125829332` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000009_0/part-00009` | 10.251.125.193, 10.251.127.191 | 0 |
| `blk_-7742258871275669707` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000126_0/part-00126` | 10.251.111.80, 10.251.30.6 | 0 |
| `blk_-7861579849076010389` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000034_0/part-00034` | 10.251.107.19, 10.251.126.227 | 0 |
| `blk_-8476126489496204177` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000024_0/part-00024` | 10.251.111.37, 10.251.126.5 | 0 |
| `blk_-2372508938534145884` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000118_0/part-00118` | 10.250.19.227, 10.251.197.161 | 0 |
| `blk_4649698331936539655` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000127_0/part-00127` | 10.251.214.67, 10.251.31.242 | 0 |
| `blk_9069966081657556515` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000031_0/part-00031` | 10.251.106.214, 10.251.214.225 | 0 |
| `blk_-2444160001954743483` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000022_0/part-00022` | 10.250.19.227, 10.251.109.236 | 0 |
| `blk_-2828839543885026602` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000021_0/part-00021` | 10.251.109.209, 10.251.65.203 | 0 |
| `blk_-3940941529251864773` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000132_0/part-00132` | 10.251.197.226, 10.251.71.16 | 0 |
| `blk_8609819439677503690` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000060_0/part-00060` | 10.251.125.174, 10.251.75.49 | 0 |
| `blk_-8281945757273050140` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000006_0/part-00006` | 10.250.17.225, 10.251.67.113 | 0 |
| `blk_2825871331379974767` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000045_0/part-00045` | 10.251.43.115, 10.251.70.211 | 0 |
| `blk_-774246298521956028` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000218_0/part-00218` | 10.251.123.33, 10.251.43.147 | 0 |
| `blk_-5649479540791129974` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000012_0/part-00012` | 10.250.19.16, 10.251.42.16 | 0 |
| `blk_-8013855621109800549` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000134_0/part-00134` | 10.250.10.6, 10.251.43.115 | 0 |
| `blk_177394776382448614` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000244_0/part-00244` | 10.251.125.237, 10.251.203.80 | 0 |
| `blk_-2398209593415798905` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000054_0/part-00054` | 10.250.5.237, 10.251.67.225 | 0 |
| `blk_-6370470857048627387` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000044_0/part-00044` | 10.250.7.96, 10.251.215.192 | 0 |
| `blk_7475438553401908725` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000130_0/part-00130` | 10.250.19.16, 10.250.7.230 | 0 |
| `blk_-8186438745381579117` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000136_0/part-00136` | 10.250.10.100, 10.251.215.192 | 0 |
| `blk_-105450231192318816` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000128_0/part-00128` | 10.251.31.180, 10.251.66.3 | 0 |
| `blk_-1385756122847916710` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000221_0/part-00221` | 10.250.14.143, 10.251.203.166 | 0 |
| `blk_2080109799967615857` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000216_0/part-00216` | 10.250.10.223, 10.251.201.204 | 0 |
| `blk_3098154771341398925` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000029_0/part-00029` | 10.251.193.224, 10.251.30.6 | 0 |
| `blk_3738211383445750914` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000050_0/part-00050` | 10.251.110.8, 10.251.42.9 | 0 |
| `blk_-4309280423174037990` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000139_0/part-00139` | 10.250.14.38, 10.251.199.225 | 0 |
| `blk_584104991019184413` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000030_0/part-00030` | 10.250.14.196, 10.251.31.242 | 0 |
| `blk_6609646171038203219` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000229_0/part-00229` | 10.251.194.245, 10.251.201.204 | 0 |
| `blk_6740695140894845846` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000137_0/part-00137` | 10.251.126.255, 10.251.67.113 | 0 |
| `blk_6835995323369082616` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000223_0/part-00223` | 10.251.110.8, 10.251.195.33 | 0 |
| `blk_-7198899606504196854` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000133_0/part-00133` | 10.251.70.211, 10.251.74.192 | 0 |
| `blk_7955481321933797799` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000037_0/part-00037` | 10.250.13.188, 10.251.67.211 | 0 |
| `blk_7956543127401791181` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000017_0/part-00017` | 10.251.195.70, 10.251.203.246 | 0 |
| `blk_8026466674001363449` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000040_0/part-00040` | 10.251.31.180, 10.251.39.209 | 0 |
| `blk_-2850818623433938591` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000038_0/part-00038` | 10.251.197.161, 10.251.66.3 | 0 |
| `blk_6227055072516687391` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000008_0/part-00008` | 10.250.10.176, 10.251.127.243 | 0 |
| `blk_1937926427363440853` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000032_0/part-00032` | 10.251.110.8, 10.251.67.113 | 0 |
| `blk_-2682415100025107597` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000138_0/part-00138` | 10.251.202.181, 10.251.66.192 | 0 |
| `blk_-2814588473145762869` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000135_0/part-00135` | 10.251.199.245, 10.251.31.180 | 0 |
| `blk_634338240549205708` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000114_0/part-00114` | 10.251.106.214, 10.251.71.16 | 0 |
| `blk_7474637556625667220` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000125_0/part-00125` | 10.251.198.196, 10.251.214.225 | 0 |
| `blk_-7911463493785581853` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000142_0/part-00142` | 10.251.111.130, 10.251.203.246 | 0 |
| `blk_967160175355936210` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000233_0/part-00233` | 10.251.31.85, 10.251.90.64 | 0 |
| `blk_3941533870808561940` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000129_0/part-00129` | 10.250.17.177, 10.251.67.113 | 0 |
| `blk_40441923226500563` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000140_0/part-00140` | 10.251.109.236, 10.251.65.237 | 0 |
| `blk_6309418393114897475` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000035_0/part-00035` | 10.251.107.19, 10.251.31.160 | 0 |
| `blk_-6790654856691185731` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000061_0/part-00061` | 10.251.111.209, 10.251.75.163 | 0 |
| `blk_8558795046002911094` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000144_0/part-00144` | 10.251.197.161, 10.251.74.227 | 0 |
| `blk_-3667585438930153850` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000014_0/part-00014` | 10.250.17.177, 10.251.30.179 | 0 |
| `blk_5479015932651254774` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000146_0/part-00146` | 10.250.15.101, 10.251.75.49 | 0 |
| `blk_6918081507126070602` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000056_0/part-00056` | 10.250.14.38, 10.251.74.227 | 0 |
| `blk_8046151943293893685` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000018_0/part-00018` | 10.251.110.68, 10.251.30.179 | 0 |
| `blk_3461505966191484945` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000027_0/part-00027` | 10.250.5.161, 10.251.31.85 | 0 |
| `blk_-4341181125838883663` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000219_0/part-00219` | 10.251.123.1, 10.251.73.220 | 0 |
| `blk_5775293647706867248` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000230_0/part-00230` | 10.251.39.144, 10.251.42.207 | 0 |
| `blk_6288745377407023095` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000280_0/part-00280` | 10.251.110.8, 10.251.42.191 | 0 |
| `blk_-3076705858972714475` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000265_0/part-00265` | 10.250.6.4, 10.251.43.21 | 0 |
| `blk_4521164666406024155` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000046_0/part-00046` | 10.251.43.147, 10.251.90.81 | 0 |
| `blk_-5835224384325268050` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000004_0/part-00004` | 10.251.199.225, 10.251.43.115 | 0 |
| `blk_-3102267849859399193` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000249_0/part-00249` | 10.251.215.50 | 0 |
| `blk_-3776330106018304557` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000064_0/part-00064` | 10.251.195.33, 10.251.74.79 | 0 |
| `blk_-3865158146925189370` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000145_0/part-00145` | 10.251.194.102, 10.251.67.225 | 0 |
| `blk_-6108780682959356968` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000245_0/part-00245` | 10.251.109.209, 10.251.66.102 | 0 |
| `blk_6597014667441085597` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000019_0/part-00019` | 10.251.125.237, 10.251.194.147 | 0 |
| `blk_-9025459096044337508` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000231_0/part-00231` | 10.251.202.209, 10.251.203.129 | 0 |
| `blk_-1477913034248457629` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000063_0/part-00063` | 10.250.15.240, 10.251.194.245 | 0 |
| `blk_1613550655988246573` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000224_0/part-00224` | 10.251.42.16 | 0 |
| `blk_2074627648226805983` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000020_0/part-00020` | 10.251.38.197, 10.251.74.192 | 0 |
| `blk_2986822300704311308` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000094_0/part-00094` | 10.251.193.224, 10.251.194.147 | 0 |
| `blk_-4390409968877773039` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000043_0/part-00043` | 10.251.42.84, 10.251.74.134 | 0 |
| `blk_-8515113585876695879` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000065_0/part-00065` | 10.251.107.242, 10.251.214.18 | 0 |
| `blk_8595954612153362607` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000016_0/part-00016` | 10.251.126.255, 10.251.70.112 | 0 |
| `blk_8665604379324651807` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000039_0/part-00039` | 10.251.122.79, 10.251.125.174 | 0 |
| `blk_-1187723472581877455` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000210_0/part-00210` | 10.251.67.225, 10.251.75.163 | 0 |
| `blk_-1548346834251221499` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000049_0/part-00049` | 10.251.199.86, 10.251.65.237 | 0 |
| `blk_-1962009378227535387` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000013_0/part-00013` | 10.251.110.196, 10.251.126.83 | 0 |
| `blk_-2804479706331551014` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000217_0/part-00217` | 10.251.67.211, 10.251.71.146 | 0 |
| `blk_4845477247445346914` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000059_0/part-00059` | 10.251.214.175, 10.251.73.220 | 0 |
| `blk_-5702477348781461354` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000262_0/part-00262` | 10.251.195.70, 10.251.42.246 | 0 |
| `blk_-5800700353547170271` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000025_0/part-00025` | 10.251.203.166, 10.251.215.70 | 0 |
| `blk_-6920587680575966964` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000248_0/part-00248` | 10.251.111.130, 10.251.91.159 | 0 |
| `blk_-7158210625015641227` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000033_0/part-00033` | 10.251.122.38, 10.251.42.16 | 0 |
| `blk_-7226303435190136642` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000001_0/part-00001` | 10.251.203.129, 10.251.215.50 | 0 |
| `blk_-8756305378882356508` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000225_0/part-00225` | 10.251.125.237, 10.251.126.83 | 0 |
| `blk_8910234103556704078` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000270_0/part-00270` | 10.251.30.101, 10.251.67.211 | 0 |
| `blk_2329219899967276279` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000000_0/part-00000` | 10.250.10.176, 10.251.201.204 | 0 |
| `blk_-2741438423499484166` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000232_0/part-00232` | 10.251.197.161, 10.251.75.228 | 0 |
| `blk_-4710160024743429585` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000222_0/part-00222` | 10.251.125.174, 10.251.195.33 | 0 |
| `blk_5491460290951785997` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000266_0/part-00266` | 10.251.202.181, 10.251.31.160 | 0 |
| `blk_-2983481749155087856` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000070_0/part-00070` | 10.251.107.196, 10.251.215.50 | 0 |
| `blk_5161501523120226002` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000211_0/part-00211` | 10.251.214.18, 10.251.39.144 | 0 |
| `blk_1325332921567711382` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000055_0/part-00055` | 10.251.31.85, 10.251.75.228 | 0 |
| `blk_-1871778753987286456` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000282_0/part-00282` | 10.250.11.100, 10.250.14.196 | 0 |
| `blk_-518459762181346762` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000228_0/part-00228` | 10.251.30.179, 10.251.90.81 | 0 |
| `blk_2391384234620089834` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000267_0/part-00267` | 10.250.14.224, 10.250.17.177 | 0 |
| `blk_3433546525085724680` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000247_0/part-00247` | 10.251.199.159, 10.251.214.67 | 0 |
| `blk_-1076549517733373559` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000069_0/part-00069` | 10.251.38.214, 10.251.74.134 | 0 |
| `blk_7427307448707327249` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000276_0/part-00276` | 10.251.123.195, 10.251.42.84 | 0 |
| `blk_2912732363095595893` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000272_0/part-00272` | 10.251.43.192, 10.251.67.4 | 0 |
| `blk_-4190491243436026170` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000053_0/part-00053` | 10.251.43.147, 10.251.75.143 | 0 |
| `blk_-4667470578821022066` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000082_0/part-00082` | 10.250.14.224, 10.251.43.21 | 0 |
| `blk_-8054782785917830420` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000281_0/part-00281` | 10.251.26.177, 10.251.42.246 | 0 |
| `blk_-8091841062813936618` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000041_0/part-00041` | 10.251.203.80, 10.251.43.210 | 0 |
| `blk_9210346052555304090` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000288_0/part-00288` | 10.250.13.240, 10.251.123.20 | 0 |
| `blk_202536106610432362` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000294_0/part-00294` | 10.251.111.228, 10.251.214.112 | 0 |
| `blk_-2869299916035182755` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000092_0/part-00092` | 10.251.122.38, 10.251.71.146 | 0 |
| `blk_-4430564173662960642` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000301_0/part-00301` | 10.251.203.4, 10.251.90.81 | 0 |
| `blk_-6586753944835843334` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000078_0/part-00078` | 10.251.38.53, 10.251.43.192 | 0 |
| `blk_-6879190567969332918` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000283_0/part-00283` | 10.250.9.207, 10.251.194.129 | 0 |
| `blk_-6931080266317198977` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000087_0/part-00087` | 10.251.123.20, 10.251.123.33 | 0 |
| `blk_7352609810175565501` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000277_0/part-00277` | 10.250.6.223, 10.251.194.213 | 0 |
| `blk_-859705208416062320` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000205_0/part-00205` | 10.250.14.224, 10.251.74.192 | 0 |
| `blk_1404945464756167309` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000051_0/part-00051` | 10.250.5.237, 10.251.42.191 | 0 |
| `blk_1448813076571847187` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000089_0/part-00089` | 10.250.15.67, 10.251.193.224 | 0 |
| `blk_1912523661436659100` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000268_0/part-00268` | 10.250.10.6, 10.251.43.147 | 0 |
| `blk_2347008916727134499` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000101_0/part-00101` | 10.251.107.50, 10.251.111.37 | 0 |
| `blk_3508011185486088738` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000312_0/part-00312` | 10.251.123.1, 10.251.195.70 | 0 |
| `blk_3880327230518840131` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000295_0/part-00295` | 10.251.43.210, 10.251.66.3 | 0 |
| `blk_3972287319691001044` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000072_0/part-00072` | 10.251.214.130, 10.251.214.32 | 0 |
| `blk_-4770972463483766707` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000085_0/part-00085` | 10.251.194.213, 10.251.74.79 | 0 |
| `blk_-6125125239494402092` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000284_0/part-00284` | 10.250.7.146, 10.251.74.134 | 0 |
| `blk_8059760933250408909` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000167_0/part-00167` | 10.250.11.194, 10.251.39.179 | 0 |
| `blk_966870748231287658` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000093_0/part-00093` | 10.250.13.240, 10.251.194.129 | 0 |
| `blk_1279101103527882599` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000287_0/part-00287` | 10.251.111.228, 10.251.122.38 | 0 |
| `blk_-2598897828182146855` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000057_0/part-00057` | 10.251.31.160, 10.251.42.246 | 0 |
| `blk_3341600009111698611` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000083_0/part-00083` | 10.250.19.16, 10.251.214.175 | 0 |
| `blk_-4002888391906787542` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000090_0/part-00090` | 10.251.110.160, 10.251.198.33 | 0 |
| `blk_4386079411548040260` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000271_0/part-00271` | 10.251.214.130, 10.251.42.16 | 0 |
| `blk_4675103097330094461` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000256_0/part-00256` | 10.251.107.19, 10.251.65.203 | 0 |
| `blk_5488900529276086615` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000275_0/part-00275` | 10.250.11.194, 10.251.126.255 | 0 |
| `blk_-7518315834509097389` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000246_0/part-00246` | 10.251.106.37, 10.251.75.143 | 0 |
| `blk_855704723224540977` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000047_0/part-00047` | 10.251.66.102, 10.251.71.146 | 0 |
| `blk_1640563687655694592` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000103_0/part-00103` | 10.251.107.242, 10.251.202.209 | 0 |
| `blk_-7851573941192785283` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000074_0/part-00074` | 10.251.195.70, 10.251.75.228 | 0 |
| `blk_-2947515393396507971` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000286_0/part-00286` | 10.250.14.224, 10.251.193.224 | 0 |
| `blk_3522654861930890662` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000269_0/part-00269` | 10.251.194.147, 10.251.214.112 | 0 |
| `blk_-4673142718163027981` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000279_0/part-00279` | 10.251.215.70, 10.251.66.192 | 0 |
| `blk_-7660193828821971101` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000273_0/part-00273` | 10.251.110.160, 10.251.214.32 | 0 |
| `blk_-9166492953238830125` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000028_0/part-00028` | 10.251.202.181, 10.251.38.53 | 0 |
| `blk_2689014683808259029` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000261_0/part-00261` | 10.251.107.227, 10.251.214.175 | 0 |
| `blk_-2973260837493784784` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000299_0/part-00299` | 10.251.214.32, 10.251.39.179 | 0 |
| `blk_3909865472571090536` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000110_0/part-00110` | 10.251.110.8, 10.251.111.80 | 0 |
| `blk_-859147788836187186` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000036_0/part-00036` | 10.251.215.70, 10.251.31.85 | 0 |
| `blk_-4393655256228529026` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000304_0/part-00304` | 10.251.42.9, 10.251.71.146 | 0 |
| `blk_-5114249203202400596` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000075_0/part-00075` | 10.251.73.188, 10.251.91.229 | 0 |
| `blk_-546318595782592836` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000100_0/part-00100` | 10.251.106.37, 10.251.111.37 | 0 |
| `blk_3363069898317139269` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000274_0/part-00274` | 10.251.39.179, 10.251.39.209 | 0 |
| `blk_8573973636575611103` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000179_0/part-00179` | 10.250.11.100, 10.251.38.197 | 0 |
| `blk_-3758406899752824535` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000278_0/part-00278` | 10.250.11.100, 10.251.194.245 | 0 |
| `blk_38865049064139660` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000310_0/part-00310` | 10.251.122.65, 10.251.90.239 | 0 |
| `blk_-4229931861869531048` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000297_0/part-00297` | 10.251.106.37, 10.251.123.132 | 0 |
| `blk_-1626340715627889482` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000236_0/part-00236` | 10.250.7.244, 10.251.127.47 | 0 |
| `blk_2221775105544933826` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000067_0/part-00067` | 10.251.111.130, 10.251.123.132 | 0 |
| `blk_4737741713837408345` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000151_0/part-00151` | 10.251.71.240, 10.251.90.134 | 0 |
| `blk_-5950249832413179417` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000184_0/part-00184` | 10.250.10.6, 10.250.6.214 | 0 |
| `blk_-8420426811026669438` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000302_0/part-00302` | 10.251.123.132, 10.251.193.175 | 0 |
| `blk_-1470784088028862059` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000311_0/part-00311` | 10.251.214.130, 10.251.90.134 | 0 |
| `blk_2366822833609442341` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000102_0/part-00102` | 10.251.123.1, 10.251.193.175 | 0 |
| `blk_-2917825689581470793` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000315_0/part-00315` | 10.250.11.85, 10.251.27.63 | 0 |
| `blk_-4046605047697127122` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000066_0/part-00066` | 10.251.106.37, 10.251.43.210 | 0 |
| `blk_-5584251724032983856` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000115_0/part-00115` | 10.251.123.132, 10.251.194.147 | 0 |
| `blk_6021477756386488418` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000290_0/part-00290` | 10.251.73.188, 10.251.90.134 | 0 |
| `blk_7268266957111394674` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000003_0/part-00003` | 10.251.203.4, 10.251.66.63 | 0 |
| `blk_8814359467727556626` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000088_0/part-00088` | 10.251.123.99, 10.251.67.4 | 0 |
| `blk_-1700929423419481113` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000220_0/part-00220` | 10.250.14.196, 10.251.39.160 | 0 |
| `blk_-2622300918011411840` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000068_0/part-00068` | 10.251.107.50, 10.251.42.207 | 0 |
| `blk_3152503487390436165` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000105_0/part-00105` | 10.250.10.213, 10.251.107.98 | 0 |
| `blk_-4365681458226063681` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000300_0/part-00300` | 10.251.111.80, 10.251.195.70 | 0 |
| `blk_-5912999755522282446` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000308_0/part-00308` | 10.250.15.67, 10.251.123.132 | 0 |
| `blk_-7559008592818043090` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000106_0/part-00106` | 10.250.15.101, 10.251.111.228 | 0 |
| `blk_-8588230903310885315` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000116_0/part-00116` | 10.251.122.65, 10.251.122.79 | 0 |
| `blk_9125494407446525156` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000292_0/part-00292` | 10.251.123.99, 10.251.66.102 | 0 |
| `blk_2619290546489140930` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000320_0/part-00320` | 10.251.195.52, 10.251.214.67 | 0 |
| `blk_-3283849791137044034` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000298_0/part-00298` | 10.250.17.177, 10.251.107.98 | 0 |
| `blk_-7686748181966193443` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000293_0/part-00293` | 10.250.9.207, 10.251.107.242 | 0 |
| `blk_-1916058035352472789` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000173_0/part-00173` | 10.251.90.81, 10.251.91.229 | 0 |
| `blk_-2278104779962392523` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000023_0/part-00023` | 10.251.30.101, 10.251.66.63 | 0 |
| `blk_-3193540685585849952` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000107_0/part-00107` | 10.251.42.84, 10.251.71.146 | 0 |
| `blk_4931832500563355889` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000307_0/part-00307` | 10.251.110.68, 10.251.31.5 | 0 |
| `blk_5133892961859808126` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000314_0/part-00314` | 10.251.31.85, 10.251.75.79 | 0 |
| `blk_-1052739769153545987` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000073_0/part-00073` | 10.251.214.67, 10.251.42.191 | 0 |
| `blk_4151093570962084251` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000285_0/part-00285` | 10.250.14.38, 10.250.7.244 | 0 |
| `blk_6717969265771639561` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000097_0/part-00097` | 10.251.110.68, 10.251.111.80 | 0 |
| `blk_-4236464647074379160` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000071_0/part-00071` | 10.251.214.112, 10.251.39.179 | 0 |
| `blk_-2498004188306609167` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000076_0/part-00076` | 10.251.107.19, 10.251.110.160 | 0 |
| `blk_5636792039332085446` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000289_0/part-00289` | 10.251.203.129, 10.251.74.79 | 0 |
| `blk_-7185891569842971867` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000080_0/part-00080` | 10.251.199.150, 10.251.74.79 | 0 |
| `blk_2583366788302307794` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000154_0/part-00154` | 10.251.26.131, 10.251.67.211 | 0 |
| `blk_-4026330115303607086` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000332_0/part-00332` | 10.251.39.160, 10.251.70.5 | 0 |
| `blk_-1577830978049349432` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000077_0/part-00077` | 10.251.25.237, 10.251.75.79 | 0 |
| `blk_6420476111425645508` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000337_0/part-00337` | 10.251.70.211, 10.251.90.239 | 0 |
| `blk_377236923047456543` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000168_0/part-00168` | 10.250.11.85, 10.251.35.1 | 0 |
| `blk_8223024669447846632` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000305_0/part-00305` | 10.250.15.67, 10.251.107.50 | 0 |
| `blk_4058804987355354315` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000306_0/part-00306` | 10.250.7.146, 10.251.89.155 | 0 |
| `blk_4856031730010032819` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000259_0/part-00259` | 10.251.197.226, 10.251.215.50 | 0 |
| `blk_-6339417867119146108` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000317_0/part-00317` | 10.251.42.9, 10.251.70.211 | 0 |
| `blk_-6714222090039882252` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000149_0/part-00149` | 10.251.110.160, 10.251.38.214 | 0 |
| `blk_4031055865781150544` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000162_0/part-00162` | 10.251.125.193, 10.251.89.155 | 0 |
| `blk_8725561728667995755` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000318_0/part-00318` | 10.250.11.85, 10.251.38.214 | 0 |
| `blk_-9084956447070300510` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000062_0/part-00062` | 10.251.125.237, 10.251.214.32 | 0 |
| `blk_-8048421706779991679` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000099_0/part-00099` | 10.251.123.1, 10.251.67.211 | 0 |
| `blk_-4525470997464616220` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000303_0/part-00303` | 10.251.107.227, 10.251.214.67 | 0 |
| `blk_872694497849122755` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000191_0/part-00191` | 10.251.106.10, 10.251.203.149 | 0 |
| `blk_-5170072115129389871` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000313_0/part-00313` | 10.251.106.10, 10.251.194.213 | 0 |
| `blk_-5493359978973542887` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000291_0/part-00291` | 10.251.107.196, 10.251.197.226 | 0 |
| `blk_-8162512552777886199` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000331_0/part-00331` | 10.250.11.53, 10.251.35.1 | 0 |
| `blk_1185079144408607775` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000328_0/part-00328` | 10.250.15.67, 10.251.26.131 | 0 |
| `blk_-6852866038059123001` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000333_0/part-00333` | 10.251.123.195, 10.251.70.112 | 0 |
| `blk_6578809109018330119` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000096_0/part-00096` | 10.250.6.214, 10.251.107.227 | 0 |
| `blk_-7130787197300034964` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000329_0/part-00329` | 10.250.15.240, 10.251.39.242 | 0 |
| `blk_2657254091763574664` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000336_0/part-00336` | 10.251.39.144, 10.251.71.97 | 0 |
| `blk_3091099087150179177` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000175_0/part-00175` | 10.250.15.198, 10.251.70.112 | 0 |
| `blk_4623571410782847630` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000342_0/part-00342` | 10.251.126.5, 10.251.71.68 | 0 |
| `blk_-568941302732430172` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000147_0/part-00147` | 10.251.38.197, 10.251.70.5 | 0 |
| `blk_2404342940449629715` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000340_0/part-00340` | 10.251.74.192, 10.251.91.229 | 0 |
| `blk_-5414183895914433338` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000212_0/part-00212` | 10.250.13.188, 10.251.71.68 | 0 |
| `blk_613065710451569222` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000325_0/part-00325` | 10.250.9.207, 10.251.122.79 | 0 |
| `blk_-67977319704233769` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000150_0/part-00150` | 10.250.9.207, 10.251.127.243 | 0 |
| `blk_-8694359045337946818` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000052_0/part-00052` | 10.250.7.230, 10.251.67.4 | 0 |
| `blk_24847521761041757` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000156_0/part-00156` | 10.250.6.214, 10.251.91.229 | 0 |
| `blk_-3238412011236244640` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000161_0/part-00161` | 10.251.30.134, 10.251.39.192 | 0 |
| `blk_-1121030458003600251` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000335_0/part-00335` | 10.251.109.209, 10.251.37.240 | 0 |
| `blk_-6827329558602881823` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000321_0/part-00321` | 10.251.199.245, 10.251.90.81 | 0 |
| `blk_7132759917805418067` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000104_0/part-00104` | 10.251.107.196, 10.251.195.52 | 0 |
| `blk_7392010738495470190` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000316_0/part-00316` | 10.250.10.100, 10.251.37.240 | 0 |
| `blk_8055148678985254863` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000157_0/part-00157` | 10.250.14.224, 10.251.39.242 | 0 |
| `blk_-3520428728308625709` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000108_0/part-00108` | 10.251.107.196, 10.251.127.191 | 0 |
| `blk_6986538622634403948` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000165_0/part-00165` | 10.250.10.100, 10.251.70.211 | 0 |
| `blk_-6035208985581955117` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000334_0/part-00334` | 10.251.109.209, 10.251.42.9 | 0 |
| `blk_6678472565712118087` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000324_0/part-00324` | 10.251.107.196, 10.251.198.33 | 0 |
| `blk_7573381576932599374` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000190_0/part-00190` | 10.251.105.189, 10.251.122.38 | 0 |
| `blk_197214010584922120` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000186_0/part-00186` | 10.251.214.67, 10.251.35.1 | 0 |
| `blk_-3253829104116368139` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000326_0/part-00326` | 10.251.202.209, 10.251.203.129 | 0 |
| `blk_-3443869707407490925` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000319_0/part-00319` | 10.251.125.193, 10.251.39.192 | 0 |
| `blk_-7689031213313245416` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000327_0/part-00327` | 10.251.105.189, 10.251.31.5 | 0 |
| `blk_3773669678166680940` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000322_0/part-00322` | 10.251.67.4, 10.251.91.159 | 0 |
| `blk_841265344082922041` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000296_0/part-00296` | 10.251.111.37, 10.251.90.81 | 0 |
| `blk_3488190436389958215` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000309_0/part-00309` | 10.251.111.228, 10.251.126.83 | 0 |
| `blk_-5358363415355511925` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000209_0/part-00209` | 10.250.7.32, 10.251.39.179 | 0 |
| `blk_5274692881287326658` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000343_0/part-00343` | 10.250.7.32, 10.251.195.70 | 0 |
| `blk_-2573988170548478524` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000155_0/part-00155` | 10.250.6.191, 10.251.71.68 | 0 |
| `blk_-5175722170941249815` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000365_0/part-00365` | 10.251.126.22, 10.251.42.9 | 0 |
| `blk_2646137657036117634` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000338_0/part-00338` | 10.251.123.33, 10.251.215.50 | 0 |
| `blk_-6232712486646639079` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000345_0/part-00345` | 10.250.13.188, 10.251.194.102 | 0 |
| `blk_-7232800529359257484` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000109_0/part-00109` | 10.251.123.33, 10.251.43.115 | 0 |
| `blk_2585807940696131461` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000351_0/part-00351` | 10.251.194.129, 10.251.39.209 | 0 |
| `blk_-7486457612086258197` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000323_0/part-00323` | 10.250.10.223, 10.251.67.225 | 0 |
| `blk_1732876454483854279` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000169_0/part-00169` | 10.251.125.174, 10.251.27.63 | 0 |
| `blk_3294295097158489623` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000163_0/part-00163` | 10.250.10.223, 10.251.38.53 | 0 |
| `blk_-6511728007995801856` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000187_0/part-00187` | 10.251.106.50, 10.251.123.195 | 0 |
| `blk_3953559589275347835` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000185_0/part-00185` | 10.250.10.144, 10.251.214.18 | 0 |
| `blk_-463158400553007438` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000170_0/part-00170` | 10.250.19.16, 10.251.202.209 | 0 |
| `blk_5614249702379360530` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000341_0/part-00341` | 10.250.7.146, 10.251.91.84 | 0 |
| `blk_38171424126089923` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000330_0/part-00330` | 10.250.10.6, 10.250.9.207 | 0 |
| `blk_-7532913187987519950` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000171_0/part-00171` | 10.251.107.196, 10.251.70.211 | 0 |
| `blk_-3318642146574997320` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000177_0/part-00177` | 10.251.193.224, 10.251.37.240 | 0 |
| `blk_-6054385392244792487` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000353_0/part-00353` | 10.251.126.5, 10.251.71.193 | 0 |
| `blk_-7545473931084004681` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000193_0/part-00193` | 10.251.199.19, 10.251.26.8 | 0 |
| `blk_-237744713088101845` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000347_0/part-00347` | 10.251.37.240, 10.251.70.37 | 0 |
| `blk_541458502420960920` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000356_0/part-00356` | 10.251.39.179, 10.251.91.15 | 0 |
| `blk_-7565306891743376805` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000372_0/part-00372` | 10.251.203.179, 10.251.38.53 | 0 |
| `blk_8465769948419925967` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000377_0/part-00377` | 10.251.203.4, 10.251.26.8 | 0 |
| `blk_1367870633219053668` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000358_0/part-00358` | 10.250.14.143, 10.251.90.239 | 0 |
| `blk_-3572954475094555028` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000195_0/part-00195` | 10.251.203.129, 10.251.70.37 | 0 |
| `blk_2455917203220074754` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000371_0/part-00371` | 10.250.15.198, 10.251.43.210 | 0 |
| `blk_-5472366049560759167` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000366_0/part-00366` | 10.251.106.50, 10.251.109.209 | 0 |
| `blk_-2449017855856764848` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000361_0/part-00361` | 10.251.31.242, 10.251.39.64 | 0 |
| `blk_-906929634502249537` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000360_0/part-00360` | 10.251.110.160, 10.251.71.97 | 0 |
| `blk_981612145312864885` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000237_0/part-00237` | 10.250.15.198, 10.251.106.214 | 0 |
| `blk_1383479986546835007` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000158_0/part-00158` | 10.251.71.97, 10.251.75.143 | 0 |
| `blk_1585681987211758961` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000113_0/part-00113` | 10.250.15.101, 10.251.122.79 | 0 |
| `blk_5678512043430077200` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000166_0/part-00166` | 10.251.126.255, 10.251.90.239 | 0 |
| `blk_3093569883558689157` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000188_0/part-00188` | 10.251.110.196, 10.251.123.132 | 0 |
| `blk_4083337788449963064` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000348_0/part-00348` | 10.251.123.195, 10.251.73.220 | 0 |
| `blk_-4916781897012753844` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000213_0/part-00213` | 10.251.199.245, 10.251.31.85 | 0 |
| `blk_7251107390153250071` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000095_0/part-00095` | 10.251.106.50, 10.251.215.50 | 0 |
| `blk_7916151846227308238` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000208_0/part-00208` | 10.251.31.5, 10.251.90.64 | 0 |
| `blk_-5671895892153119162` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000350_0/part-00350` | 10.251.199.245, 10.251.71.68 | 0 |
| `blk_-669219608853085168` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000346_0/part-00346` | 10.250.11.53 | 0 |
| `blk_2044409903741578562` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000363_0/part-00363` | 10.251.109.209, 10.251.91.84 | 0 |
| `blk_2327492205667109903` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000374_0/part-00374` | 10.251.38.197, 10.251.39.242 | 0 |
| `blk_4036267293724823480` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000370_0/part-00370` | 10.250.15.240, 10.251.127.47 | 0 |
| `blk_6055889596475060317` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000354_0/part-00354` | 10.250.10.144, 10.251.199.225 | 0 |
| `blk_3878871310819862093` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000364_0/part-00364` | 10.251.110.196, 10.251.91.159 | 0 |
| `blk_-2667064017324592326` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000148_0/part-00148` | 10.251.194.147, 10.251.39.144 | 0 |
| `blk_8518644188978882261` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000344_0/part-00344` | 10.251.110.8, 10.251.215.16 | 0 |
| `blk_8155607960458082281` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000189_0/part-00189` | 10.251.110.8, 10.251.123.1 | 0 |
| `blk_2163147564840591022` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000352_0/part-00352` | 10.250.11.53, 10.251.71.240 | 0 |
| `blk_6525645224298470536` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000192_0/part-00192` | 10.250.10.144, 10.251.71.193 | 0 |
| `blk_4289625875479909480` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000206_0/part-00206` | 10.250.10.213, 10.251.39.242 | 0 |
| `blk_3239380279521454817` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000176_0/part-00176` | 10.250.11.194, 10.251.110.160 | 0 |
| `blk_-2870732575704612495` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000204_0/part-00204` | 10.251.39.144, 10.251.91.15 | 0 |
| `blk_6121212718624278543` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000253_0/part-00253` | 10.250.6.191, 10.251.194.102 | 0 |
| `blk_-3407920531979306897` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000375_0/part-00375` | 10.250.5.161, 10.251.126.83 | 0 |
| `blk_-6795505241391858229` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000367_0/part-00367` | 10.251.107.196, 10.251.121.224 | 0 |
| `blk_7182298358730791197` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000369_0/part-00369` | 10.250.15.101, 10.250.6.191 | 0 |
| `blk_-5584875496006139565` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000091_0/part-00091` | 10.250.6.4, 10.251.121.224 | 0 |
| `blk_-2864044183052157382` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000368_0/part-00368` | 10.250.10.213, 10.250.13.188 | 0 |
| `blk_3043753876829603164` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000152_0/part-00152` | 10.250.5.237, 10.251.39.64 | 0 |
| `blk_-1268704127005579599` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000357_0/part-00357` | 10.251.203.149, 10.251.90.64 | 0 |
| `blk_2931242832797339515` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000183_0/part-00183` | 10.250.11.53, 10.251.31.242 | 0 |
| `blk_-3901171193985872487` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000384_0/part-00384` | 10.251.197.161, 10.251.215.50 | 0 |
| `blk_4082071052002133707` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000159_0/part-00159` | 10.250.13.240, 10.251.39.209 | 0 |
| `blk_-3534835244149147539` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000196_0/part-00196` | 10.250.15.240, 10.251.74.227 | 0 |
| `blk_-3616976684127351139` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000349_0/part-00349` | 10.250.11.194 | 0 |
| `blk_4712938983108943253` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000359_0/part-00359` | 10.251.199.19, 10.251.66.102 | 0 |
| `blk_-8144145466118512432` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000355_0/part-00355` | 10.251.27.63, 10.251.70.211 | 0 |
| `blk_8294878850903324975` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000178_0/part-00178` | 10.251.126.255, 10.251.39.160 | 0 |
| `blk_-1442035270681298125` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000215_0/part-00215` | 10.251.199.19 | 0 |
| `blk_-4013352108974757296` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000182_0/part-00182` | 10.250.10.176, 10.251.73.188 | 0 |
| `blk_8624561841460699343` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000180_0/part-00180` | 10.251.71.240, 10.251.91.229 | 0 |
| `blk_4139091197886383131` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000198_0/part-00198` | 10.250.7.146, 10.251.73.188 | 0 |
| `blk_8135292087230413975` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000378_0/part-00378` | 10.250.10.176, 10.250.13.188 | 0 |
| `blk_4566277459864535342` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000362_0/part-00362` | 10.251.106.50, 10.251.110.196 | 0 |
| `blk_-7713388298429426884` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000381_0/part-00381` | 10.250.7.96, 10.251.214.225 | 0 |
| `blk_-3046627542547536054` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000251_0/part-00251` | 10.250.7.96, 10.251.70.37 | 0 |
| `blk_5661848627083847913` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000257_0/part-00257` | 10.251.126.22, 10.251.91.159 | 0 |
| `blk_-7194027365856463559` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000197_0/part-00197` | 10.250.5.161, 10.250.6.214 | 0 |
| `blk_-6704225939638932097` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000383_0/part-00383` | 10.251.194.147, 10.251.91.32 | 0 |
| `blk_1239909128373552502` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000174_0/part-00174` | 10.251.215.16, 10.251.91.32 | 0 |
| `blk_-8741334127422264486` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000376_0/part-00376` | 10.250.15.240, 10.251.199.150 | 0 |
| `blk_7804208990899438071` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000258_0/part-00258` | 10.251.199.150, 10.251.90.239 | 0 |
| `blk_-5143286617671754617` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000172_0/part-00172` | 10.250.18.114, 10.251.109.209 | 0 |
| `blk_8183815269486695335` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000181_0/part-00181` | 10.251.198.196, 10.251.91.84 | 0 |
| `blk_6192538903136651854` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000238_0/part-00238` | 10.251.197.161 | 0 |
| `blk_7340860866556817481` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000382_0/part-00382` | 10.251.26.177 | 0 |
| `blk_1780513736067213693` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000194_0/part-00194` | 10.251.106.214, 10.251.29.239 | 0 |
| `blk_6402803854715743550` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000388_0/part-00388` | 10.251.203.80, 10.251.29.239 | 0 |
| `blk_8257277173936543986` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000385_0/part-00385` | 10.251.199.159, 10.251.70.211 | 0 |
| `blk_2602936878633669240` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000380_0/part-00380` | 10.251.194.102, 10.251.42.9 | 0 |
| `blk_3647098096660331647` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000373_0/part-00373` | 10.251.39.160, 10.251.74.192 | 0 |
| `blk_-4147447158944475729` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000207_0/part-00207` | 10.251.106.50, 10.251.38.197 | 0 |
| `blk_6733725894073150811` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000386_0/part-00386` | 10.251.123.20, 10.251.198.196 | 0 |
| `blk_6745860875456203032` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000201_0/part-00201` | 10.251.203.179, 10.251.26.177 | 0 |
| `blk_-6441832264813778096` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000379_0/part-00379` | 10.251.106.214, 10.251.195.70 | 0 |
| `blk_-9032182111118378964` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000202_0/part-00202` | 10.250.14.143, 10.251.29.239 | 0 |
| `blk_-994288696663095595` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000203_0/part-00203` | 10.251.197.226, 10.251.38.53 | 0 |
| `blk_5579327064488516122` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000235_0/part-00235` | 10.251.194.102, 10.251.91.15 | 0 |
| `blk_-2064366892727746021` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000242_0/part-00242` | 10.251.199.159, 10.251.199.225 | 0 |
| `blk_7359269325129318656` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000164_0/part-00164` | 10.251.109.236 | 0 |
| `blk_8554832644956889891` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000387_0/part-00387` | 10.250.15.67, 10.251.202.209 | 0 |
| `blk_328595803100712652` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000339_0/part-00339` | 10.251.109.236, 10.251.26.177 | 0 |
| `blk_6005203045226256208` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000214_0/part-00214` | 10.250.7.96, 10.251.198.196 | 0 |
| `blk_6687412102242126380` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000389_0/part-00389` | 10.250.13.240, 10.251.215.50 | 0 |
| `blk_5167166035808601451` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000239_0/part-00239` | 10.250.15.67 | 0 |
| `blk_268467721592087811` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000153_0/part-00153` | 10.251.43.192, 10.251.91.159 | 0 |
| `blk_1472526959254300198` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000392_0/part-00392` | 10.250.15.101, 10.251.127.47 | 0 |
| `blk_8643402937543183462` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000160_0/part-00160` | 10.251.25.237 | 0 |
| `blk_7031936135774406790` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000254_0/part-00254` | 10.250.6.223, 10.251.194.245 | 0 |
| `blk_-4255221183685916173` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000264_0/part-00264` | 10.251.29.239 | 0 |
| `blk_-7837133872976053297` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000260_0/part-00260` | 10.251.127.47 | 0 |
| `blk_-8991056407448381439` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000394_0/part-00394` | 10.251.199.86 | 0 |
| `blk_-2126554733521224025` | unknown | `/user/root/rand/_temporary/_task_200811092030_0001_m_000200_0/part-00200` | 10.251.199.86 | 0 |
| `blk_1191349968162171906` | unknown | `(no allocateBlock seen)` | 10.251.25.237 | 0 |
| `blk_1568365807861288923` | unknown | `(no allocateBlock seen)` | 10.251.203.179 | 0 |
| `blk_1242317965473535156` | unknown | `(no allocateBlock seen)` | 10.251.203.179 | 0 |
| `blk_4989545155659808163` | unknown | `(no allocateBlock seen)` | 10.250.6.4 | 0 |
| `blk_7146075618207518599` | unknown | `(no allocateBlock seen)` | 10.250.6.4 | 0 |

---
*Report generated automatically from `hdfs_datanode.log`.*