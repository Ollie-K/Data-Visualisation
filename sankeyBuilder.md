---
id: litvis

narrative-schemas:
  - ../../narrative-schemas/project.yml

elm:
  dependencies:
    gicentre/elm-vegalite: latest
    gicentre/elm-vega: latest
    gicentre/tidy: latest
---

@import "../../css/datavis.less"

```elm {l=hidden}
import Bitwise
import Dict
import Set
import Tidy exposing (..)
import Vega as V
import VegaLite exposing (..)
```

```elm {l=hidden}
ssData : Table
ssData =
    """Officer-defined ethnicity,Age range,Object of search,Outcome,count
Asian,10-17,Anything to threaten or harm anyone,A no further action disposal,140
Asian,10-17,Anything to threaten or harm anyone,Arrest,7
Asian,10-17,Anything to threaten or harm anyone,Community resolution,1
Asian,10-17,Articles for use in criminal damage,A no further action disposal,24
Asian,10-17,Articles for use in criminal damage,Arrest,1
Asian,10-17,Articles for use in criminal damage,Community resolution,2
Asian,10-17,Controlled drugs,A no further action disposal,3147
Asian,10-17,Controlled drugs,Arrest,282
Asian,10-17,Controlled drugs,Caution (simple or conditional),9
Asian,10-17,Controlled drugs,Community resolution,97
Asian,10-17,Controlled drugs,Penalty Notice for Disorder,12
Asian,10-17,Controlled drugs,Summons / charged by post,63
Asian,10-17,Evidence of offences under the Act,A no further action disposal,209
Asian,10-17,Evidence of offences under the Act,Arrest,42
Asian,10-17,Evidence of offences under the Act,Community resolution,4
Asian,10-17,Evidence of offences under the Act,Penalty Notice for Disorder,1
Asian,10-17,Evidence of offences under the Act,Summons / charged by post,1
Asian,10-17,Firearms,A no further action disposal,16
Asian,10-17,Firearms,Arrest,8
Asian,10-17,Fireworks,A no further action disposal,68
Asian,10-17,Fireworks,Arrest,1
Asian,10-17,Fireworks,Community resolution,2
Asian,10-17,Offensive weapons,A no further action disposal,1317
Asian,10-17,Offensive weapons,Arrest,164
Asian,10-17,Offensive weapons,Community resolution,7
Asian,10-17,Offensive weapons,Penalty Notice for Disorder,4
Asian,10-17,Offensive weapons,Summons / charged by post,9
Asian,10-17,Stolen goods,A no further action disposal,496
Asian,10-17,Stolen goods,Arrest,84
Asian,10-17,Stolen goods,Community resolution,8
Asian,10-17,Stolen goods,Penalty Notice for Disorder,3
Asian,10-17,Stolen goods,Summons / charged by post,4
Asian,18-24,Anything to threaten or harm anyone,A no further action disposal,445
Asian,18-24,Anything to threaten or harm anyone,Arrest,21
Asian,18-24,Anything to threaten or harm anyone,Community resolution,19
Asian,18-24,Anything to threaten or harm anyone,Penalty Notice for Disorder,9
Asian,18-24,Anything to threaten or harm anyone,Summons / charged by post,1
Asian,18-24,Articles for use in criminal damage,A no further action disposal,26
Asian,18-24,Articles for use in criminal damage,Arrest,2
Asian,18-24,Articles for use in criminal damage,Community resolution,1
Asian,18-24,Articles for use in criminal damage,Penalty Notice for Disorder,1
Asian,18-24,Controlled drugs,A no further action disposal,14247
Asian,18-24,Controlled drugs,Arrest,1208
Asian,18-24,Controlled drugs,Caution (simple or conditional),12
Asian,18-24,Controlled drugs,Community resolution,2363
Asian,18-24,Controlled drugs,Penalty Notice for Disorder,1036
Asian,18-24,Controlled drugs,Summons / charged by post,335
Asian,18-24,Evidence of offences under the Act,A no further action disposal,308
Asian,18-24,Evidence of offences under the Act,Arrest,45
Asian,18-24,Evidence of offences under the Act,Community resolution,25
Asian,18-24,Evidence of offences under the Act,Penalty Notice for Disorder,13
Asian,18-24,Evidence of offences under the Act,Summons / charged by post,3
Asian,18-24,Firearms,A no further action disposal,81
Asian,18-24,Firearms,Arrest,8
Asian,18-24,Firearms,Community resolution,5
Asian,18-24,Firearms,Penalty Notice for Disorder,2
Asian,18-24,Fireworks,A no further action disposal,35
Asian,18-24,Fireworks,Community resolution,2
Asian,18-24,Fireworks,Penalty Notice for Disorder,3
Asian,18-24,Offensive weapons,A no further action disposal,2270
Asian,18-24,Offensive weapons,Arrest,261
Asian,18-24,Offensive weapons,Community resolution,91
Asian,18-24,Offensive weapons,Penalty Notice for Disorder,63
Asian,18-24,Offensive weapons,Summons / charged by post,21
Asian,18-24,Stolen goods,A no further action disposal,715
Asian,18-24,Stolen goods,Arrest,132
Asian,18-24,Stolen goods,Community resolution,45
Asian,18-24,Stolen goods,Penalty Notice for Disorder,40
Asian,18-24,Stolen goods,Summons / charged by post,9
Asian,25-34,Anything to threaten or harm anyone,A no further action disposal,222
Asian,25-34,Anything to threaten or harm anyone,Arrest,10
Asian,25-34,Anything to threaten or harm anyone,Community resolution,13
Asian,25-34,Anything to threaten or harm anyone,Penalty Notice for Disorder,2
Asian,25-34,Anything to threaten or harm anyone,Summons / charged by post,1
Asian,25-34,Articles for use in criminal damage,A no further action disposal,9
Asian,25-34,Articles for use in criminal damage,Arrest,6
Asian,25-34,Controlled drugs,A no further action disposal,7472
Asian,25-34,Controlled drugs,Arrest,978
Asian,25-34,Controlled drugs,Caution (simple or conditional),9
Asian,25-34,Controlled drugs,Community resolution,1421
Asian,25-34,Controlled drugs,Penalty Notice for Disorder,615
Asian,25-34,Controlled drugs,Summons / charged by post,226
Asian,25-34,Evidence of offences under the Act,A no further action disposal,257
Asian,25-34,Evidence of offences under the Act,Arrest,41
Asian,25-34,Evidence of offences under the Act,Community resolution,13
Asian,25-34,Evidence of offences under the Act,Penalty Notice for Disorder,5
Asian,25-34,Evidence of offences under the Act,Summons / charged by post,1
Asian,25-34,Firearms,A no further action disposal,47
Asian,25-34,Firearms,Arrest,10
Asian,25-34,Firearms,Community resolution,4
Asian,25-34,Firearms,Penalty Notice for Disorder,5
Asian,25-34,Firearms,Summons / charged by post,1
Asian,25-34,Fireworks,A no further action disposal,12
Asian,25-34,Fireworks,Community resolution,2
Asian,25-34,Fireworks,Penalty Notice for Disorder,3
Asian,25-34,Offensive weapons,A no further action disposal,1033
Asian,25-34,Offensive weapons,Arrest,181
Asian,25-34,Offensive weapons,Caution (simple or conditional),1
Asian,25-34,Offensive weapons,Community resolution,27
Asian,25-34,Offensive weapons,Penalty Notice for Disorder,23
Asian,25-34,Offensive weapons,Summons / charged by post,8
Asian,25-34,Stolen goods,A no further action disposal,546
Asian,25-34,Stolen goods,Arrest,128
Asian,25-34,Stolen goods,Caution (simple or conditional),1
Asian,25-34,Stolen goods,Community resolution,34
Asian,25-34,Stolen goods,Penalty Notice for Disorder,18
Asian,25-34,Stolen goods,Summons / charged by post,13
Asian,over 34,Anything to threaten or harm anyone,A no further action disposal,75
Asian,over 34,Anything to threaten or harm anyone,Arrest,6
Asian,over 34,Anything to threaten or harm anyone,Community resolution,1
Asian,over 34,Anything to threaten or harm anyone,Penalty Notice for Disorder,2
Asian,over 34,Articles for use in criminal damage,A no further action disposal,14
Asian,over 34,Articles for use in criminal damage,Arrest,7
Asian,over 34,Controlled drugs,A no further action disposal,3401
Asian,over 34,Controlled drugs,Arrest,592
Asian,over 34,Controlled drugs,Caution (simple or conditional),7
Asian,over 34,Controlled drugs,Community resolution,370
Asian,over 34,Controlled drugs,Penalty Notice for Disorder,132
Asian,over 34,Controlled drugs,Summons / charged by post,80
Asian,over 34,Evidence of offences under the Act,A no further action disposal,279
Asian,over 34,Evidence of offences under the Act,Arrest,67
Asian,over 34,Evidence of offences under the Act,Community resolution,9
Asian,over 34,Evidence of offences under the Act,Penalty Notice for Disorder,3
Asian,over 34,Evidence of offences under the Act,Summons / charged by post,3
Asian,over 34,Firearms,A no further action disposal,22
Asian,over 34,Firearms,Arrest,9
Asian,over 34,Firearms,Community resolution,1
Asian,over 34,Firearms,Penalty Notice for Disorder,1
Asian,over 34,Fireworks,A no further action disposal,2
Asian,over 34,Fireworks,Penalty Notice for Disorder,1
Asian,over 34,Offensive weapons,A no further action disposal,387
Asian,over 34,Offensive weapons,Arrest,148
Asian,over 34,Offensive weapons,Community resolution,16
Asian,over 34,Offensive weapons,Penalty Notice for Disorder,4
Asian,over 34,Offensive weapons,Summons / charged by post,3
Asian,over 34,Stolen goods,A no further action disposal,520
Asian,over 34,Stolen goods,Arrest,145
Asian,over 34,Stolen goods,Community resolution,39
Asian,over 34,Stolen goods,Penalty Notice for Disorder,12
Asian,over 34,Stolen goods,Summons / charged by post,7
Asian,under 10,Controlled drugs,A no further action disposal,9
Asian,under 10,Controlled drugs,Penalty Notice for Disorder,1
Asian,under 10,Controlled drugs,Summons / charged by post,1
Asian,under 10,Fireworks,Penalty Notice for Disorder,1
Asian,under 10,Offensive weapons,A no further action disposal,3
Black,10-17,Anything to threaten or harm anyone,A no further action disposal,563
Black,10-17,Anything to threaten or harm anyone,Arrest,40
Black,10-17,Anything to threaten or harm anyone,Community resolution,4
Black,10-17,Anything to threaten or harm anyone,Summons / charged by post,5
Black,10-17,Articles for use in criminal damage,A no further action disposal,71
Black,10-17,Articles for use in criminal damage,Arrest,11
Black,10-17,Articles for use in criminal damage,Community resolution,3
Black,10-17,Controlled drugs,A no further action disposal,7042
Black,10-17,Controlled drugs,Arrest,944
Black,10-17,Controlled drugs,Caution (simple or conditional),29
Black,10-17,Controlled drugs,Community resolution,220
Black,10-17,Controlled drugs,Penalty Notice for Disorder,21
Black,10-17,Controlled drugs,Summons / charged by post,157
Black,10-17,Evidence of offences under the Act,A no further action disposal,721
Black,10-17,Evidence of offences under the Act,Arrest,128
Black,10-17,Evidence of offences under the Act,Caution (simple or conditional),1
Black,10-17,Evidence of offences under the Act,Community resolution,2
Black,10-17,Evidence of offences under the Act,Penalty Notice for Disorder,2
Black,10-17,Evidence of offences under the Act,Summons / charged by post,8
Black,10-17,Firearms,A no further action disposal,83
Black,10-17,Firearms,Arrest,16
Black,10-17,Fireworks,A no further action disposal,365
Black,10-17,Fireworks,Arrest,18
Black,10-17,Fireworks,Community resolution,12
Black,10-17,Fireworks,Penalty Notice for Disorder,3
Black,10-17,Fireworks,Summons / charged by post,1
Black,10-17,Offensive weapons,A no further action disposal,6858
Black,10-17,Offensive weapons,Arrest,966
Black,10-17,Offensive weapons,Caution (simple or conditional),1
Black,10-17,Offensive weapons,Community resolution,42
Black,10-17,Offensive weapons,Penalty Notice for Disorder,11
Black,10-17,Offensive weapons,Summons / charged by post,38
Black,10-17,Stolen goods,A no further action disposal,2464
Black,10-17,Stolen goods,Arrest,589
Black,10-17,Stolen goods,Caution (simple or conditional),3
Black,10-17,Stolen goods,Community resolution,45
Black,10-17,Stolen goods,Penalty Notice for Disorder,10
Black,10-17,Stolen goods,Summons / charged by post,25
Black,18-24,Anything to threaten or harm anyone,A no further action disposal,862
Black,18-24,Anything to threaten or harm anyone,Arrest,62
Black,18-24,Anything to threaten or harm anyone,Community resolution,46
Black,18-24,Anything to threaten or harm anyone,Penalty Notice for Disorder,20
Black,18-24,Anything to threaten or harm anyone,Summons / charged by post,5
Black,18-24,Articles for use in criminal damage,A no further action disposal,44
Black,18-24,Articles for use in criminal damage,Arrest,11
Black,18-24,Articles for use in criminal damage,Community resolution,5
Black,18-24,Articles for use in criminal damage,Penalty Notice for Disorder,1
Black,18-24,Articles for use in criminal damage,Summons / charged by post,1
Black,18-24,Controlled drugs,A no further action disposal,20461
Black,18-24,Controlled drugs,Arrest,2757
Black,18-24,Controlled drugs,Caution (simple or conditional),11
Black,18-24,Controlled drugs,Community resolution,3365
Black,18-24,Controlled drugs,Penalty Notice for Disorder,1179
Black,18-24,Controlled drugs,Summons / charged by post,479
Black,18-24,Evidence of offences under the Act,A no further action disposal,640
Black,18-24,Evidence of offences under the Act,Arrest,102
Black,18-24,Evidence of offences under the Act,Community resolution,36
Black,18-24,Evidence of offences under the Act,Penalty Notice for Disorder,19
Black,18-24,Evidence of offences under the Act,Summons / charged by post,5
Black,18-24,Firearms,A no further action disposal,183
Black,18-24,Firearms,Arrest,41
Black,18-24,Firearms,Community resolution,4
Black,18-24,Firearms,Penalty Notice for Disorder,1
Black,18-24,Fireworks,A no further action disposal,56
Black,18-24,Fireworks,Arrest,4
Black,18-24,Fireworks,Community resolution,4
Black,18-24,Fireworks,Penalty Notice for Disorder,4
Black,18-24,Fireworks,Summons / charged by post,2
Black,18-24,Offensive weapons,A no further action disposal,7209
Black,18-24,Offensive weapons,Arrest,1141
Black,18-24,Offensive weapons,Community resolution,232
Black,18-24,Offensive weapons,Penalty Notice for Disorder,176
Black,18-24,Offensive weapons,Summons / charged by post,69
Black,18-24,Stolen goods,A no further action disposal,1881
Black,18-24,Stolen goods,Arrest,418
Black,18-24,Stolen goods,Community resolution,107
Black,18-24,Stolen goods,Penalty Notice for Disorder,59
Black,18-24,Stolen goods,Summons / charged by post,32
Black,25-34,Anything to threaten or harm anyone,A no further action disposal,407
Black,25-34,Anything to threaten or harm anyone,Arrest,25
Black,25-34,Anything to threaten or harm anyone,Community resolution,34
Black,25-34,Anything to threaten or harm anyone,Penalty Notice for Disorder,10
Black,25-34,Anything to threaten or harm anyone,Summons / charged by post,10
Black,25-34,Articles for use in criminal damage,A no further action disposal,17
Black,25-34,Articles for use in criminal damage,Arrest,5
Black,25-34,Articles for use in criminal damage,Community resolution,5
Black,25-34,Articles for use in criminal damage,Penalty Notice for Disorder,1
Black,25-34,Controlled drugs,A no further action disposal,11339
Black,25-34,Controlled drugs,Arrest,2050
Black,25-34,Controlled drugs,Caution (simple or conditional),9
Black,25-34,Controlled drugs,Community resolution,2072
Black,25-34,Controlled drugs,Penalty Notice for Disorder,728
Black,25-34,Controlled drugs,Summons / charged by post,347
Black,25-34,Evidence of offences under the Act,A no further action disposal,430
Black,25-34,Evidence of offences under the Act,Arrest,96
Black,25-34,Evidence of offences under the Act,Community resolution,19
Black,25-34,Evidence of offences under the Act,Penalty Notice for Disorder,12
Black,25-34,Evidence of offences under the Act,Summons / charged by post,4
Black,25-34,Firearms,A no further action disposal,95
Black,25-34,Firearms,Arrest,52
Black,25-34,Firearms,Community resolution,5
Black,25-34,Firearms,Penalty Notice for Disorder,3
Black,25-34,Fireworks,A no further action disposal,10
Black,25-34,Fireworks,Arrest,2
Black,25-34,Fireworks,Community resolution,1
Black,25-34,Fireworks,Penalty Notice for Disorder,1
Black,25-34,Offensive weapons,A no further action disposal,2724
Black,25-34,Offensive weapons,Arrest,600
Black,25-34,Offensive weapons,Community resolution,106
Black,25-34,Offensive weapons,Penalty Notice for Disorder,69
Black,25-34,Offensive weapons,Summons / charged by post,34
Black,25-34,Stolen goods,A no further action disposal,1039
Black,25-34,Stolen goods,Arrest,360
Black,25-34,Stolen goods,Community resolution,66
Black,25-34,Stolen goods,Penalty Notice for Disorder,46
Black,25-34,Stolen goods,Summons / charged by post,22
Black,over 34,Anything to threaten or harm anyone,A no further action disposal,151
Black,over 34,Anything to threaten or harm anyone,Arrest,17
Black,over 34,Anything to threaten or harm anyone,Community resolution,9
Black,over 34,Anything to threaten or harm anyone,Penalty Notice for Disorder,4
Black,over 34,Articles for use in criminal damage,A no further action disposal,29
Black,over 34,Articles for use in criminal damage,Arrest,12
Black,over 34,Articles for use in criminal damage,Community resolution,2
Black,over 34,Controlled drugs,A no further action disposal,7389
Black,over 34,Controlled drugs,Arrest,1411
Black,over 34,Controlled drugs,Caution (simple or conditional),5
Black,over 34,Controlled drugs,Community resolution,953
Black,over 34,Controlled drugs,Penalty Notice for Disorder,270
Black,over 34,Controlled drugs,Summons / charged by post,125
Black,over 34,Evidence of offences under the Act,A no further action disposal,613
Black,over 34,Evidence of offences under the Act,Arrest,218
Black,over 34,Evidence of offences under the Act,Community resolution,10
Black,over 34,Evidence of offences under the Act,Penalty Notice for Disorder,5
Black,over 34,Evidence of offences under the Act,Summons / charged by post,5
Black,over 34,Firearms,A no further action disposal,77
Black,over 34,Firearms,Arrest,10
Black,over 34,Firearms,Community resolution,1
Black,over 34,Firearms,Penalty Notice for Disorder,1
Black,over 34,Fireworks,A no further action disposal,6
Black,over 34,Fireworks,Arrest,2
Black,over 34,Fireworks,Community resolution,1
Black,over 34,Offensive weapons,A no further action disposal,1130
Black,over 34,Offensive weapons,Arrest,456
Black,over 34,Offensive weapons,Community resolution,41
Black,over 34,Offensive weapons,Penalty Notice for Disorder,15
Black,over 34,Offensive weapons,Summons / charged by post,7
Black,over 34,Stolen goods,A no further action disposal,1178
Black,over 34,Stolen goods,Arrest,500
Black,over 34,Stolen goods,Caution (simple or conditional),2
Black,over 34,Stolen goods,Community resolution,86
Black,over 34,Stolen goods,Penalty Notice for Disorder,22
Black,over 34,Stolen goods,Summons / charged by post,27
Black,under 10,Anything to threaten or harm anyone,A no further action disposal,1
Black,under 10,Controlled drugs,A no further action disposal,7
Black,under 10,Controlled drugs,Arrest,1
Black,under 10,Controlled drugs,Community resolution,1
Black,under 10,Controlled drugs,Penalty Notice for Disorder,1
Black,under 10,Evidence of offences under the Act,A no further action disposal,1
Black,under 10,Evidence of offences under the Act,Arrest,1
Black,under 10,Offensive weapons,A no further action disposal,4
Black,under 10,Stolen goods,A no further action disposal,1
Other,10-17,Anything to threaten or harm anyone,A no further action disposal,66
Other,10-17,Anything to threaten or harm anyone,Arrest,2
Other,10-17,Articles for use in criminal damage,A no further action disposal,9
Other,10-17,Articles for use in criminal damage,Arrest,1
Other,10-17,Articles for use in criminal damage,Community resolution,1
Other,10-17,Controlled drugs,A no further action disposal,953
Other,10-17,Controlled drugs,Arrest,93
Other,10-17,Controlled drugs,Caution (simple or conditional),3
Other,10-17,Controlled drugs,Community resolution,36
Other,10-17,Controlled drugs,Penalty Notice for Disorder,2
Other,10-17,Controlled drugs,Summons / charged by post,18
Other,10-17,Evidence of offences under the Act,A no further action disposal,75
Other,10-17,Evidence of offences under the Act,Arrest,7
Other,10-17,Firearms,A no further action disposal,5
Other,10-17,Fireworks,A no further action disposal,55
Other,10-17,Fireworks,Arrest,1
Other,10-17,Fireworks,Community resolution,2
Other,10-17,Offensive weapons,A no further action disposal,472
Other,10-17,Offensive weapons,Arrest,52
Other,10-17,Offensive weapons,Community resolution,5
Other,10-17,Offensive weapons,Penalty Notice for Disorder,3
Other,10-17,Offensive weapons,Summons / charged by post,5
Other,10-17,Stolen goods,A no further action disposal,251
Other,10-17,Stolen goods,Arrest,32
Other,10-17,Stolen goods,Community resolution,10
Other,10-17,Stolen goods,Penalty Notice for Disorder,1
Other,10-17,Stolen goods,Summons / charged by post,1
Other,18-24,Anything to threaten or harm anyone,A no further action disposal,153
Other,18-24,Anything to threaten or harm anyone,Arrest,8
Other,18-24,Anything to threaten or harm anyone,Community resolution,8
Other,18-24,Anything to threaten or harm anyone,Penalty Notice for Disorder,4
Other,18-24,Articles for use in criminal damage,A no further action disposal,7
Other,18-24,Articles for use in criminal damage,Arrest,2
Other,18-24,Controlled drugs,A no further action disposal,3272
Other,18-24,Controlled drugs,Arrest,309
Other,18-24,Controlled drugs,Caution (simple or conditional),1
Other,18-24,Controlled drugs,Community resolution,538
Other,18-24,Controlled drugs,Penalty Notice for Disorder,175
Other,18-24,Controlled drugs,Summons / charged by post,62
Other,18-24,Evidence of offences under the Act,A no further action disposal,122
Other,18-24,Evidence of offences under the Act,Arrest,16
Other,18-24,Evidence of offences under the Act,Community resolution,7
Other,18-24,Evidence of offences under the Act,Penalty Notice for Disorder,1
Other,18-24,Firearms,A no further action disposal,17
Other,18-24,Firearms,Arrest,6
Other,18-24,Firearms,Community resolution,1
Other,18-24,Fireworks,A no further action disposal,28
Other,18-24,Fireworks,Community resolution,1
Other,18-24,Offensive weapons,A no further action disposal,813
Other,18-24,Offensive weapons,Arrest,107
Other,18-24,Offensive weapons,Community resolution,35
Other,18-24,Offensive weapons,Penalty Notice for Disorder,20
Other,18-24,Offensive weapons,Summons / charged by post,9
Other,18-24,Stolen goods,A no further action disposal,359
Other,18-24,Stolen goods,Arrest,92
Other,18-24,Stolen goods,Community resolution,17
Other,18-24,Stolen goods,Penalty Notice for Disorder,12
Other,18-24,Stolen goods,Summons / charged by post,5
Other,25-34,Anything to threaten or harm anyone,A no further action disposal,54
Other,25-34,Anything to threaten or harm anyone,Arrest,4
Other,25-34,Anything to threaten or harm anyone,Community resolution,2
Other,25-34,Articles for use in criminal damage,A no further action disposal,6
Other,25-34,Articles for use in criminal damage,Arrest,1
Other,25-34,Controlled drugs,A no further action disposal,1564
Other,25-34,Controlled drugs,Arrest,232
Other,25-34,Controlled drugs,Caution (simple or conditional),1
Other,25-34,Controlled drugs,Community resolution,300
Other,25-34,Controlled drugs,Penalty Notice for Disorder,98
Other,25-34,Controlled drugs,Summons / charged by post,38
Other,25-34,Evidence of offences under the Act,A no further action disposal,102
Other,25-34,Evidence of offences under the Act,Arrest,21
Other,25-34,Evidence of offences under the Act,Community resolution,7
Other,25-34,Evidence of offences under the Act,Penalty Notice for Disorder,1
Other,25-34,Evidence of offences under the Act,Summons / charged by post,1
Other,25-34,Firearms,A no further action disposal,11
Other,25-34,Firearms,Arrest,2
Other,25-34,Firearms,Penalty Notice for Disorder,1
Other,25-34,Fireworks,A no further action disposal,5
Other,25-34,Fireworks,Arrest,1
Other,25-34,Offensive weapons,A no further action disposal,291
Other,25-34,Offensive weapons,Arrest,53
Other,25-34,Offensive weapons,Community resolution,22
Other,25-34,Offensive weapons,Penalty Notice for Disorder,6
Other,25-34,Offensive weapons,Summons / charged by post,1
Other,25-34,Stolen goods,A no further action disposal,271
Other,25-34,Stolen goods,Arrest,91
Other,25-34,Stolen goods,Community resolution,12
Other,25-34,Stolen goods,Penalty Notice for Disorder,5
Other,25-34,Stolen goods,Summons / charged by post,5
Other,over 34,Anything to threaten or harm anyone,A no further action disposal,17
Other,over 34,Articles for use in criminal damage,A no further action disposal,2
Other,over 34,Articles for use in criminal damage,Arrest,1
Other,over 34,Controlled drugs,A no further action disposal,684
Other,over 34,Controlled drugs,Arrest,105
Other,over 34,Controlled drugs,Community resolution,100
Other,over 34,Controlled drugs,Penalty Notice for Disorder,22
Other,over 34,Controlled drugs,Summons / charged by post,8
Other,over 34,Evidence of offences under the Act,A no further action disposal,88
Other,over 34,Evidence of offences under the Act,Arrest,23
Other,over 34,Evidence of offences under the Act,Community resolution,2
Other,over 34,Firearms,A no further action disposal,5
Other,over 34,Firearms,Arrest,3
Other,over 34,Firearms,Penalty Notice for Disorder,2
Other,over 34,Offensive weapons,A no further action disposal,122
Other,over 34,Offensive weapons,Arrest,42
Other,over 34,Offensive weapons,Community resolution,5
Other,over 34,Offensive weapons,Penalty Notice for Disorder,1
Other,over 34,Offensive weapons,Summons / charged by post,1
Other,over 34,Stolen goods,A no further action disposal,175
Other,over 34,Stolen goods,Arrest,56
Other,over 34,Stolen goods,Caution (simple or conditional),1
Other,over 34,Stolen goods,Community resolution,8
Other,over 34,Stolen goods,Penalty Notice for Disorder,3
Other,over 34,Stolen goods,Summons / charged by post,1
Other,under 10,Controlled drugs,A no further action disposal,4
Other,under 10,Controlled drugs,Community resolution,2
Other,under 10,Evidence of offences under the Act,A no further action disposal,1
Other,under 10,Offensive weapons,A no further action disposal,1
Other,under 10,Stolen goods,A no further action disposal,1
White,10-17,Anything to threaten or harm anyone,A no further action disposal,366
White,10-17,Anything to threaten or harm anyone,Arrest,13
White,10-17,Anything to threaten or harm anyone,Caution (simple or conditional),1
White,10-17,Anything to threaten or harm anyone,Community resolution,3
White,10-17,Anything to threaten or harm anyone,Penalty Notice for Disorder,1
White,10-17,Anything to threaten or harm anyone,Summons / charged by post,6
White,10-17,Articles for use in criminal damage,A no further action disposal,261
White,10-17,Articles for use in criminal damage,Arrest,21
White,10-17,Articles for use in criminal damage,Community resolution,8
White,10-17,Articles for use in criminal damage,Summons / charged by post,1
White,10-17,Controlled drugs,A no further action disposal,8161
White,10-17,Controlled drugs,Arrest,759
White,10-17,Controlled drugs,Caution (simple or conditional),43
White,10-17,Controlled drugs,Community resolution,377
White,10-17,Controlled drugs,Penalty Notice for Disorder,23
White,10-17,Controlled drugs,Summons / charged by post,270
White,10-17,Evidence of offences under the Act,A no further action disposal,1211
White,10-17,Evidence of offences under the Act,Arrest,177
White,10-17,Evidence of offences under the Act,Community resolution,16
White,10-17,Evidence of offences under the Act,Penalty Notice for Disorder,4
White,10-17,Evidence of offences under the Act,Summons / charged by post,9
White,10-17,Firearms,A no further action disposal,38
White,10-17,Firearms,Arrest,8
White,10-17,Firearms,Community resolution,1
White,10-17,Fireworks,A no further action disposal,228
White,10-17,Fireworks,Arrest,5
White,10-17,Fireworks,Community resolution,10
White,10-17,Fireworks,Penalty Notice for Disorder,1
White,10-17,Fireworks,Summons / charged by post,2
White,10-17,Offensive weapons,A no further action disposal,3357
White,10-17,Offensive weapons,Arrest,407
White,10-17,Offensive weapons,Caution (simple or conditional),2
White,10-17,Offensive weapons,Community resolution,28
White,10-17,Offensive weapons,Penalty Notice for Disorder,7
White,10-17,Offensive weapons,Summons / charged by post,26
White,10-17,Stolen goods,A no further action disposal,2651
White,10-17,Stolen goods,Arrest,427
White,10-17,Stolen goods,Caution (simple or conditional),2
White,10-17,Stolen goods,Community resolution,59
White,10-17,Stolen goods,Penalty Notice for Disorder,14
White,10-17,Stolen goods,Summons / charged by post,23
White,18-24,Anything to threaten or harm anyone,A no further action disposal,584
White,18-24,Anything to threaten or harm anyone,Arrest,21
White,18-24,Anything to threaten or harm anyone,Community resolution,38
White,18-24,Anything to threaten or harm anyone,Penalty Notice for Disorder,12
White,18-24,Anything to threaten or harm anyone,Summons / charged by post,4
White,18-24,Articles for use in criminal damage,A no further action disposal,106
White,18-24,Articles for use in criminal damage,Arrest,34
White,18-24,Articles for use in criminal damage,Community resolution,11
White,18-24,Articles for use in criminal damage,Penalty Notice for Disorder,7
White,18-24,Articles for use in criminal damage,Summons / charged by post,5
White,18-24,Controlled drugs,A no further action disposal,18144
White,18-24,Controlled drugs,Arrest,1941
White,18-24,Controlled drugs,Caution (simple or conditional),25
White,18-24,Controlled drugs,Community resolution,4514
White,18-24,Controlled drugs,Penalty Notice for Disorder,1015
White,18-24,Controlled drugs,Summons / charged by post,310
White,18-24,Evidence of offences under the Act,A no further action disposal,1392
White,18-24,Evidence of offences under the Act,Arrest,231
White,18-24,Evidence of offences under the Act,Community resolution,71
White,18-24,Evidence of offences under the Act,Penalty Notice for Disorder,30
White,18-24,Evidence of offences under the Act,Summons / charged by post,12
White,18-24,Firearms,A no further action disposal,64
White,18-24,Firearms,Arrest,26
White,18-24,Firearms,Penalty Notice for Disorder,2
White,18-24,Firearms,Summons / charged by post,1
White,18-24,Fireworks,A no further action disposal,69
White,18-24,Fireworks,Arrest,1
White,18-24,Fireworks,Community resolution,6
White,18-24,Fireworks,Penalty Notice for Disorder,3
White,18-24,Fireworks,Summons / charged by post,1
White,18-24,Offensive weapons,A no further action disposal,3175
White,18-24,Offensive weapons,Arrest,495
White,18-24,Offensive weapons,Community resolution,148
White,18-24,Offensive weapons,Penalty Notice for Disorder,78
White,18-24,Offensive weapons,Summons / charged by post,17
White,18-24,Stolen goods,A no further action disposal,2845
White,18-24,Stolen goods,Arrest,551
White,18-24,Stolen goods,Caution (simple or conditional),1
White,18-24,Stolen goods,Community resolution,183
White,18-24,Stolen goods,Penalty Notice for Disorder,96
White,18-24,Stolen goods,Summons / charged by post,41
White,25-34,Anything to threaten or harm anyone,A no further action disposal,341
White,25-34,Anything to threaten or harm anyone,Arrest,26
White,25-34,Anything to threaten or harm anyone,Community resolution,12
White,25-34,Anything to threaten or harm anyone,Penalty Notice for Disorder,3
White,25-34,Anything to threaten or harm anyone,Summons / charged by post,1
White,25-34,Articles for use in criminal damage,A no further action disposal,85
White,25-34,Articles for use in criminal damage,Arrest,41
White,25-34,Articles for use in criminal damage,Community resolution,6
White,25-34,Articles for use in criminal damage,Penalty Notice for Disorder,3
White,25-34,Articles for use in criminal damage,Summons / charged by post,2
White,25-34,Controlled drugs,A no further action disposal,13679
White,25-34,Controlled drugs,Arrest,2383
White,25-34,Controlled drugs,Caution (simple or conditional),8
White,25-34,Controlled drugs,Community resolution,2686
White,25-34,Controlled drugs,Penalty Notice for Disorder,694
White,25-34,Controlled drugs,Summons / charged by post,277
White,25-34,Evidence of offences under the Act,A no further action disposal,1582
White,25-34,Evidence of offences under the Act,Arrest,384
White,25-34,Evidence of offences under the Act,Caution (simple or conditional),1
White,25-34,Evidence of offences under the Act,Community resolution,53
White,25-34,Evidence of offences under the Act,Penalty Notice for Disorder,21
White,25-34,Evidence of offences under the Act,Summons / charged by post,12
White,25-34,Firearms,A no further action disposal,101
White,25-34,Firearms,Arrest,29
White,25-34,Firearms,Community resolution,6
White,25-34,Firearms,Penalty Notice for Disorder,9
White,25-34,Firearms,Summons / charged by post,2
White,25-34,Fireworks,A no further action disposal,17
White,25-34,Fireworks,Arrest,5
White,25-34,Fireworks,Community resolution,1
White,25-34,Fireworks,Penalty Notice for Disorder,1
White,25-34,Fireworks,Summons / charged by post,1
White,25-34,Offensive weapons,A no further action disposal,2034
White,25-34,Offensive weapons,Arrest,541
White,25-34,Offensive weapons,Caution (simple or conditional),2
White,25-34,Offensive weapons,Community resolution,51
White,25-34,Offensive weapons,Penalty Notice for Disorder,37
White,25-34,Offensive weapons,Summons / charged by post,17
White,25-34,Stolen goods,A no further action disposal,3161
White,25-34,Stolen goods,Arrest,913
White,25-34,Stolen goods,Caution (simple or conditional),2
White,25-34,Stolen goods,Community resolution,169
White,25-34,Stolen goods,Penalty Notice for Disorder,97
White,25-34,Stolen goods,Summons / charged by post,55
White,over 34,Anything to threaten or harm anyone,A no further action disposal,190
White,over 34,Anything to threaten or harm anyone,Arrest,18
White,over 34,Anything to threaten or harm anyone,Community resolution,3
White,over 34,Anything to threaten or harm anyone,Penalty Notice for Disorder,4
White,over 34,Anything to threaten or harm anyone,Summons / charged by post,2
White,over 34,Articles for use in criminal damage,A no further action disposal,91
White,over 34,Articles for use in criminal damage,Arrest,51
White,over 34,Articles for use in criminal damage,Community resolution,6
White,over 34,Articles for use in criminal damage,Penalty Notice for Disorder,2
White,over 34,Articles for use in criminal damage,Summons / charged by post,2
White,over 34,Controlled drugs,A no further action disposal,13100
White,over 34,Controlled drugs,Arrest,1980
White,over 34,Controlled drugs,Caution (simple or conditional),7
White,over 34,Controlled drugs,Community resolution,1228
White,over 34,Controlled drugs,Penalty Notice for Disorder,259
White,over 34,Controlled drugs,Summons / charged by post,142
White,over 34,Evidence of offences under the Act,A no further action disposal,2028
White,over 34,Evidence of offences under the Act,Arrest,520
White,over 34,Evidence of offences under the Act,Community resolution,38
White,over 34,Evidence of offences under the Act,Penalty Notice for Disorder,16
White,over 34,Evidence of offences under the Act,Summons / charged by post,10
White,over 34,Firearms,A no further action disposal,82
White,over 34,Firearms,Arrest,31
White,over 34,Firearms,Community resolution,1
White,over 34,Firearms,Penalty Notice for Disorder,3
White,over 34,Fireworks,A no further action disposal,13
White,over 34,Fireworks,Arrest,2
White,over 34,Fireworks,Community resolution,3
White,over 34,Fireworks,Summons / charged by post,1
White,over 34,Offensive weapons,A no further action disposal,1700
White,over 34,Offensive weapons,Arrest,604
White,over 34,Offensive weapons,Community resolution,36
White,over 34,Offensive weapons,Penalty Notice for Disorder,12
White,over 34,Offensive weapons,Summons / charged by post,10
White,over 34,Stolen goods,A no further action disposal,3735
White,over 34,Stolen goods,Arrest,1101
White,over 34,Stolen goods,Community resolution,260
White,over 34,Stolen goods,Penalty Notice for Disorder,75
White,over 34,Stolen goods,Summons / charged by post,69
White,under 10,Controlled drugs,A no further action disposal,6
White,under 10,Controlled drugs,Arrest,2
White,under 10,Controlled drugs,Community resolution,1
White,under 10,Controlled drugs,Penalty Notice for Disorder,2
White,under 10,Evidence of offences under the Act,A no further action disposal,1
White,under 10,Offensive weapons,A no further action disposal,5
White,under 10,Stolen goods,A no further action disposal,6
White,under 10,Stolen goods,Arrest,3""" |>fromCSV

```

```elm {l=hidden}
cumulativeSearches : Table
cumulativeSearches = """Age,Officer-Defined Ethnicity,Searches
1,Asian,0.00
2,Asian,0.00
3,Asian,0.00
4,Asian,0.00
5,Asian,0.00
6,Asian,0.00
7,Asian,0.00
8,Asian,0.00
9,Asian,0.00
10,Asian,0.03
11,Asian,0.06
12,Asian,0.08
13,Asian,0.11
14,Asian,0.14
15,Asian,0.16
16,Asian,0.19
17,Asian,0.22
18,Asian,0.36
19,Asian,0.49
20,Asian,0.63
21,Asian,0.77
22,Asian,0.91
23,Asian,1.05
24,Asian,1.18
25,Asian,1.22
26,Asian,1.26
27,Asian,1.30
28,Asian,1.34
29,Asian,1.38
30,Asian,1.41
31,Asian,1.45
32,Asian,1.49
33,Asian,1.53
34,Asian,1.57
35,Asian,1.58
36,Asian,1.58
37,Asian,1.59
38,Asian,1.60
39,Asian,1.60
40,Asian,1.61
41,Asian,1.62
42,Asian,1.63
43,Asian,1.63
44,Asian,1.64
45,Asian,1.65
46,Asian,1.65
47,Asian,1.66
48,Asian,1.67
49,Asian,1.68
50,Asian,1.68
51,Asian,1.69
52,Asian,1.70
53,Asian,1.71
54,Asian,1.71
55,Asian,1.72
56,Asian,1.73
57,Asian,1.73
58,Asian,1.74
59,Asian,1.75
60,Asian,1.76
61,Asian,1.76
62,Asian,1.77
63,Asian,1.78
64,Asian,1.78
65,Asian,1.79
66,Asian,1.80
67,Asian,1.81
68,Asian,1.81
69,Asian,1.82
70,Asian,1.83
71,Asian,1.83
72,Asian,1.84
73,Asian,1.85
74,Asian,1.86
75,Asian,1.86
76,Asian,1.87
77,Asian,1.88
78,Asian,1.89
79,Asian,1.89
80,Asian,1.90
81,Asian,1.91
82,Asian,1.91
83,Asian,1.92
84,Asian,1.93
85,Asian,1.94
86,Asian,1.94
87,Asian,1.95
88,Asian,1.96
89,Asian,1.96
90,Asian,1.97
91,Asian,1.98
92,Asian,1.99
93,Asian,1.99
94,Asian,2.00
95,Asian,2.01
96,Asian,2.01
97,Asian,2.02
98,Asian,2.03
99,Asian,2.04
100,Asian,2.04
1,Black,0.00
2,Black,0.00
3,Black,0.00
4,Black,0.00
5,Black,0.00
6,Black,0.00
7,Black,0.00
8,Black,0.00
9,Black,0.00
10,Black,0.12
11,Black,0.24
12,Black,0.36
13,Black,0.47
14,Black,0.59
15,Black,0.71
16,Black,0.83
17,Black,0.95
18,Black,1.36
19,Black,1.78
20,Black,2.20
21,Black,2.62
22,Black,3.03
23,Black,3.45
24,Black,3.87
25,Black,4.01
26,Black,4.16
27,Black,4.31
28,Black,4.46
29,Black,4.60
30,Black,4.75
31,Black,4.90
32,Black,5.05
33,Black,5.19
34,Black,5.34
35,Black,5.37
36,Black,5.39
37,Black,5.42
38,Black,5.44
39,Black,5.47
40,Black,5.49
41,Black,5.52
42,Black,5.54
43,Black,5.57
44,Black,5.59
45,Black,5.62
46,Black,5.64
47,Black,5.67
48,Black,5.69
49,Black,5.72
50,Black,5.75
51,Black,5.77
52,Black,5.80
53,Black,5.82
54,Black,5.85
55,Black,5.87
56,Black,5.90
57,Black,5.92
58,Black,5.95
59,Black,5.97
60,Black,6.00
61,Black,6.02
62,Black,6.05
63,Black,6.07
64,Black,6.10
65,Black,6.13
66,Black,6.15
67,Black,6.18
68,Black,6.20
69,Black,6.23
70,Black,6.25
71,Black,6.28
72,Black,6.30
73,Black,6.33
74,Black,6.35
75,Black,6.38
76,Black,6.40
77,Black,6.43
78,Black,6.46
79,Black,6.48
80,Black,6.51
81,Black,6.53
82,Black,6.56
83,Black,6.58
84,Black,6.61
85,Black,6.63
86,Black,6.66
87,Black,6.68
88,Black,6.71
89,Black,6.73
90,Black,6.76
91,Black,6.78
92,Black,6.81
93,Black,6.84
94,Black,6.86
95,Black,6.89
96,Black,6.91
97,Black,6.94
98,Black,6.96
99,Black,6.99
100,Black,7.01
1,Other,0.00
2,Other,0.00
3,Other,0.00
4,Other,0.00
5,Other,0.00
6,Other,0.00
7,Other,0.00
8,Other,0.00
9,Other,0.00
10,Other,0.05
11,Other,0.09
12,Other,0.14
13,Other,0.19
14,Other,0.23
15,Other,0.28
16,Other,0.33
17,Other,0.37
18,Other,0.56
19,Other,0.75
20,Other,0.94
21,Other,1.13
22,Other,1.32
23,Other,1.51
24,Other,1.70
25,Other,1.76
26,Other,1.81
27,Other,1.86
28,Other,1.91
29,Other,1.96
30,Other,2.01
31,Other,2.06
32,Other,2.11
33,Other,2.16
34,Other,2.21
35,Other,2.22
36,Other,2.23
37,Other,2.24
38,Other,2.25
39,Other,2.26
40,Other,2.27
41,Other,2.27
42,Other,2.28
43,Other,2.29
44,Other,2.30
45,Other,2.31
46,Other,2.32
47,Other,2.33
48,Other,2.34
49,Other,2.35
50,Other,2.35
51,Other,2.36
52,Other,2.37
53,Other,2.38
54,Other,2.39
55,Other,2.40
56,Other,2.41
57,Other,2.42
58,Other,2.43
59,Other,2.43
60,Other,2.44
61,Other,2.45
62,Other,2.46
63,Other,2.47
64,Other,2.48
65,Other,2.49
66,Other,2.50
67,Other,2.51
68,Other,2.51
69,Other,2.52
70,Other,2.53
71,Other,2.54
72,Other,2.55
73,Other,2.56
74,Other,2.57
75,Other,2.58
76,Other,2.59
77,Other,2.59
78,Other,2.60
79,Other,2.61
80,Other,2.62
81,Other,2.63
82,Other,2.64
83,Other,2.65
84,Other,2.66
85,Other,2.67
86,Other,2.67
87,Other,2.68
88,Other,2.69
89,Other,2.70
90,Other,2.71
91,Other,2.72
92,Other,2.73
93,Other,2.74
94,Other,2.75
95,Other,2.75
96,Other,2.76
97,Other,2.77
98,Other,2.78
99,Other,2.79
100,Other,2.80
1,White,0.00
2,White,0.00
3,White,0.00
4,White,0.00
5,White,0.00
6,White,0.00
7,White,0.00
8,White,0.00
9,White,0.00
10,White,0.04
11,White,0.08
12,White,0.12
13,White,0.16
14,White,0.20
15,White,0.24
16,White,0.28
17,White,0.32
18,White,0.41
19,White,0.50
20,White,0.59
21,White,0.68
22,White,0.77
23,White,0.86
24,White,0.95
25,White,0.98
26,White,1.01
27,White,1.04
28,White,1.07
29,White,1.10
30,White,1.14
31,White,1.17
32,White,1.20
33,White,1.23
34,White,1.26
35,White,1.27
36,White,1.28
37,White,1.29
38,White,1.30
39,White,1.31
40,White,1.32
41,White,1.33
42,White,1.34
43,White,1.35
44,White,1.36
45,White,1.37
46,White,1.38
47,White,1.39
48,White,1.40
49,White,1.41
50,White,1.42
51,White,1.43
52,White,1.44
53,White,1.45
54,White,1.46
55,White,1.47
56,White,1.48
57,White,1.49
58,White,1.50
59,White,1.51
60,White,1.52
61,White,1.53
62,White,1.54
63,White,1.55
64,White,1.56
65,White,1.57
66,White,1.58
67,White,1.59
68,White,1.60
69,White,1.60
70,White,1.61
71,White,1.62
72,White,1.63
73,White,1.64
74,White,1.65
75,White,1.66
76,White,1.67
77,White,1.68
78,White,1.69
79,White,1.70
80,White,1.71
81,White,1.72
82,White,1.73
83,White,1.74
84,White,1.75
85,White,1.76
86,White,1.77
87,White,1.78
88,White,1.79
89,White,1.80
90,White,1.81
91,White,1.82
92,White,1.83
93,White,1.84
94,White,1.85
95,White,1.86
96,White,1.87
97,White,1.88
98,White,1.89
99,White,1.90
100,White,1.91
""" |> fromCSV
```

```elm {l=hidden}
asianT : Table
asianT =
    ssData
        |> filterRows "Officer-defined ethnicity" (\v -> v == "Asian")
        |> filterRows "Age range" (\v -> v == "10-17")

blackT : Table
blackT =
    ssData
        |> filterRows "Officer-defined ethnicity" (\v -> v == "Black")
        |> filterRows "Age range" (\v -> v == "10-17")

otherT : Table
otherT =
    ssData
        |> filterRows "Officer-defined ethnicity" (\v -> v == "Other")
        |> filterRows "Age range" (\v -> v == "10-17")


whiteT : Table
whiteT =
    ssData
        |> filterRows "Officer-defined ethnicity" (\v -> v == "White")
        |> filterRows "Age range" (\v -> v == "10-17")

```

```elm {l=hidden}
asianY : Table
asianY =
    ssData
        |> filterRows "Officer-defined ethnicity" (\v -> v == "Asian")
        |> filterRows "Age range" (\v -> v == "18-24")

blackY : Table
blackY =
    ssData
        |> filterRows "Officer-defined ethnicity" (\v -> v == "Black")
        |> filterRows "Age range" (\v -> v == "18-24")

otherY : Table
otherY =
    ssData
        |> filterRows "Officer-defined ethnicity" (\v -> v == "Other")
        |> filterRows "Age range" (\v -> v == "18-24")


whiteY : Table
whiteY =
    ssData
        |> filterRows "Officer-defined ethnicity" (\v -> v == "White")
        |> filterRows "Age range" (\v -> v == "18-24")

```

```elm {l=hidden}
asianM : Table
asianM =
    ssData
        |> filterRows "Officer-defined ethnicity" (\v -> v == "Asian")
        |> filterRows "Age range" (\v -> v == "25-34")

blackM : Table
blackM =
    ssData
        |> filterRows "Officer-defined ethnicity" (\v -> v == "Black")
        |> filterRows "Age range" (\v -> v == "25-34")

otherM : Table
otherM =
    ssData
        |> filterRows "Officer-defined ethnicity" (\v -> v == "Other")
        |> filterRows "Age range" (\v -> v == "25-34")


whiteM : Table
whiteM =
    ssData
        |> filterRows "Officer-defined ethnicity" (\v -> v == "White")
        |> filterRows "Age range" (\v -> v == "25-34")

```

```elm {l=hidden}
asianO : Table
asianO =
    ssData
        |> filterRows "Officer-defined ethnicity" (\v -> v == "Asian")
        |> filterRows "Age range" (\v -> v == "over 34")

blackO : Table
blackO =
    ssData
        |> filterRows "Officer-defined ethnicity" (\v -> v == "Black")
        |> filterRows "Age range" (\v -> v == "over 34")

otherO : Table
otherO =
    ssData
        |> filterRows "Officer-defined ethnicity" (\v -> v == "Other")
        |> filterRows "Age range" (\v -> v == "over 34")


whiteO : Table
whiteO =
    ssData
        |> filterRows "Officer-defined ethnicity" (\v -> v == "White")
        |> filterRows "Age range" (\v -> v == "over 34")

```

```elm {l=hidden}
-- All from Jo Wood.
-- Record for storing Sankey customisation options.


type alias Sankey =
    { data : V.DataTable
    , dataSource : String
    , oName : String
    , dName : String
    , vName : String
    , width : Float
    , height : Float
    , oLabel : String
    , dLabel : String
    , cScale : List V.ScaleProperty
    }
```

```elm {l=hidden}
-- Sankey diagram builder


sankeyBuilder : Sankey -> Spec
sankeyBuilder sankey =
    let
        ds =
            V.dataSource <|
                (sankey.data
                    |> V.transform
                        [ V.trFormula ("datum['" ++ sankey.oName ++ "']") "stk1"
                        , V.trFormula ("datum['" ++ sankey.dName ++ "']") "stk2"
                        , V.trFormula ("datum['" ++ sankey.vName ++ "']") "size"
                        ]
                )
                    :: [ V.data "nodes" [ V.daSource sankey.dataSource ]
                            |> V.transform
                                [ V.trFilter (V.expr "!groupSelector || groupSelector.stk1 == datum.stk1 || groupSelector.stk2 == datum.stk2")
                                , V.trFormula "datum.stk1+datum.stk2" "key"
                                , V.trFoldAs [ V.field "stk1", V.field "stk2" ] "stack" "grpId"
                                , V.trFormula "datum.stack == 'stk1' ? datum.stk1+' '+datum.stk2 : datum.stk2+' '+datum.stk1" "sortField"
                                , V.trStack
                                    [ V.stField (V.field "size")
                                    , V.stSort [ ( V.field "sortField", V.descend ) ]
                                    , V.stGroupBy [ V.field "stack" ]
                                    ]
                                , V.trFormula "(datum.y0+datum.y1)/2" "yc"
                                ]
                       , V.data "groups" [ V.daSource "nodes" ]
                            |> V.transform
                                [ V.trAggregate
                                    [ V.agFields [ V.field "size" ]
                                    , V.agOps [ V.opSum ]
                                    , V.agGroupBy [ V.field "stack", V.field "grpId" ]
                                    , V.agAs [ "total" ]
                                    ]
                                , V.trStack
                                    [ V.stField (V.field "total")
                                    , V.stSort [ ( V.field "grpId", V.descend ) ]
                                    , V.stGroupBy [ V.field "stack" ]
                                    ]
                                , V.trFormula "scale('yScale', datum.y0)" "scaledY0"
                                , V.trFormula "scale('yScale', datum.y1)" "scaledY1"
                                , V.trFormula "datum.stack == 'stk1'" "rightLabel"
                                , V.trFormula "datum.total/domain('yScale')[1]" "percentage"
                                ]
                       , V.data "destinationNodes" [ V.daSource "nodes" ]
                            |> V.transform [ V.trFilter (V.expr "datum.stack == 'stk2'") ]
                       , V.data "edges" [ V.daSource "nodes" ]
                            |> V.transform
                                [ V.trFilter (V.expr "datum.stack == 'stk1'")
                                , V.trLookup "destinationNodes" (V.field "key") [ V.field "key" ] [ V.luAs [ "target" ] ]
                                , V.trLinkPath
                                    [ V.lpOrient V.orHorizontal
                                    , V.lpShape V.lsDiagonal
                                    , V.lpSourceX (V.fExpr "scale('xScale', 'stk1') + bandwidth('xScale')")
                                    , V.lpSourceY (V.fExpr "scale('yScale', datum.yc)")
                                    , V.lpTargetX (V.fExpr "scale('xScale', 'stk2')")
                                    , V.lpTargetY (V.fExpr "scale('yScale', datum.target.yc)")
                                    ]
                                , V.trFormula "range('yScale')[0]-scale('yScale', datum.size)" "strokeWidth"
                                , V.trFormula "datum.size/domain('yScale')[1]" "percentage"
                                ]
                       ]

        si =
            V.signals
                << V.signal "groupHover"
                    [ V.siValue (V.vStr "{}")
                    , V.siOn
                        [ V.evHandler [ V.esSelector (V.str "@groupMark:mouseover") ]
                            [ V.evUpdate "{stk1:datum.stack=='stk1' && datum.grpId, stk2:datum.stack=='stk2' && datum.grpId}" ]
                        , V.evHandler [ V.esSelector (V.str "mouseout") ] [ V.evUpdate "{}" ]
                        ]
                    ]
                << V.signal "groupSelector"
                    [ V.siValue V.vFalse
                    , V.siOn
                        [ V.evHandler [ V.esSelector (V.str "@groupMark:click!") ]
                            [ V.evUpdate "{stack:datum.stack, stk1:datum.stack=='stk1' && datum.grpId, stk2:datum.stack=='stk2' && datum.grpId}" ]
                        , V.evHandler
                            [ V.esObject [ V.esType V.etClick, V.esMarkName "groupReset" ]
                            , V.esObject [ V.esType V.etDblClick ]
                            ]
                            [ V.evUpdate "false" ]
                        ]
                    ]

        sc =
            V.scales
                << V.scale "xScale"
                    [ V.scType V.scBand
                    , V.scDomain (V.doStrs (V.strs [ "stk1", "stk2" ]))
                    , V.scRange V.raWidth
                    , V.scPaddingInner (V.num 0.95)
                    , V.scPaddingOuter (V.num 0.05)
                    ]
                << V.scale "yScale"
                    [ V.scType V.scLinear
                    , V.scDomain (V.doData [ V.daDataset "nodes", V.daField (V.field "y1") ])
                    , V.scRange V.raHeight
                    ]
                << V.scale "cScale" sankey.cScale
                << V.scale "stackNames"
                    [ V.scType V.scOrdinal
                    , V.scDomain (V.doStrs (V.strs [ "stk1", "stk2" ]))
                    , V.scRange (V.raStrs [ sankey.oLabel, sankey.dLabel ])
                    ]

        ax =
            V.axes
                << V.axis "xScale"
                    V.siTop
                    [ V.axDomain V.false
                    , V.axTicks V.false
                    , V.axLabelPadding (V.num 10)
                    , V.axEncode
                        [ ( V.aeLabels
                          , [ V.enUpdate [ V.maText [ V.vScale "stackNames", V.vField (V.field "value") ] ] ]
                          )
                        ]
                    ]
                << V.axis "yScale"
                    V.siLeft
                    [ V.axDomain V.false
                    , V.axTicks V.false
                    , V.axLabels V.false
                    ]

        mk =
            V.marks
                -- Left and right stacks representing each transition combination between the two states.
                << V.mark V.rect
                    [ V.mFrom [ V.srData (V.str "nodes") ]
                    , V.mEncode
                        [ V.enEnter
                            [ V.maWidth [ V.vScale "xScale", V.vBand (V.num 1) ]
                            , V.maX [ V.vScale "xScale", V.vField (V.field "stack") ]
                            , V.maY [ V.vScale "yScale", V.vField (V.field "y0") ]
                            , V.maY2 [ V.vScale "yScale", V.vField (V.field "y1") ]
                            , V.maFill [ V.vScale "cScale", V.vField (V.field "grpId") ]
                            , V.maStroke [ V.black ]
                            , V.maStrokeWidth [ V.vNum 0.2 ]
                            ]
                        ]
                    ]
                -- Left and right stack groups (group of nodes with common origin or destination).
                << V.mark V.rect
                    [ V.mName "groupMark"
                    , V.mFrom [ V.srData (V.str "groups") ]
                    , V.mEncode
                        [ V.enEnter
                            [ V.maFillOpacity [ V.vNum 0 ]
                            , V.maFill [ V.vStr "white" ]
                            , V.maWidth [ V.vScale "xScale", V.vBand (V.num 1) ]
                            , V.maStroke [ V.vStr "white" ]
                            , V.maStrokeWidth [ V.vNum 2 ]
                            , V.maCornerRadius [ V.vNum 2 ]
                            ]
                        , V.enUpdate
                            [ V.maX [ V.vScale "xScale", V.vField (V.field "stack") ]
                            , V.maY [ V.vField (V.field "scaledY0") ]
                            , V.maY2 [ V.vField (V.field "scaledY1") ]
                            , V.maTooltip [ V.vSignal "datum.grpId + '   ' + format(datum.total, ',.0f') + '   (' + format(datum.percentage, '.1%') + ')'" ]
                            ]
                        , V.enHover
                            [ V.maStrokeOpacity [ V.vNum 1 ]
                            ]
                        ]
                    ]
                -- The Sankey flow lines.
                << V.mark V.path
                    [ V.mName "edgeMark"
                    , V.mFrom [ V.srData (V.str "edges") ]
                    , V.mClip (V.clEnabled V.true)
                    , V.mEncode
                        [ V.enUpdate
                            [ V.maStroke
                                [ V.ifElse "groupSelector && groupSelector.stack=='stk1'"
                                    [ V.vScale "cScale", V.vField (V.field "stk2") ]
                                    [ V.vScale "cScale", V.vField (V.field "grpId") ]
                                ]
                            , V.maStrokeWidth [ V.vField (V.field "strokeWidth") ]
                            , V.maPath [ V.vField (V.field "path") ]
                            , V.maStrokeOpacity [ V.vSignal "!groupSelector && (groupHover.stk1 == datum.stk1 || groupHover.stk2 == datum.stk2) ? 0.9 : 0.3" ]
                            , V.maZIndex [ V.vSignal "!groupSelector && (groupHover.stk1 == datum.stk1 || groupHover.stk2 == datum.stk2) ? 1 : 0" ]
                            , V.maTooltip [ V.vSignal "datum.stk1 + ' → ' + datum.stk2 + '    ' + format(datum.size, ',.0f') + '   (' + format(datum.percentage, '.1%') + ')'" ]
                            ]
                        , V.enHover [ V.maStrokeOpacity [ V.vNum 1 ] ]
                        ]
                    ]
                -- Group labels.
                << V.mark V.text
                    [ V.mFrom [ V.srData (V.str "groups") ]
                    , V.mInteractive V.false
                    , V.mEncode
                        [ V.enUpdate
                            [ V.maX [ V.vSignal "scale('xScale', datum.stack) + (datum.rightLabel ? bandwidth('xScale') + 8 : -8)" ]
                            , V.maY [ V.vSignal "(datum.scaledY0 + datum.scaledY1)/2" ]
                            , V.maAlign [ V.vSignal "datum.rightLabel ? 'left' : 'right'" ]
                            , V.maBaseline [ V.vMiddle ]
                            , V.maFontWeight [ V.vStr "bold" ]
                            , V.maText [ V.vSignal "abs(datum.scaledY0-datum.scaledY1) > 13 ? datum.grpId : ''" ]
                            ]
                        ]
                    ]
    in
    V.toVega
        [ V.width sankey.width, V.height sankey.height, ds, si [], sc [], ax [], mk [] ]
```

```elm {l=hidden}
categoricalDomainMap : List ( String, String ) -> List V.ScaleProperty
categoricalDomainMap scaleDomainPairs =
    -- Convenience function for creating a custom categorical colour scheme
    -- from a list of category-colour tuples.
    let
        ( domain, range ) =
            List.unzip scaleDomainPairs
    in
    [ V.scType V.scOrdinal
    , V.scDomain (V.doStrs (V.strs domain))
    , V.scRange (V.raStrs range)
    ]
```
