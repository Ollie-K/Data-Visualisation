---
follows: sankeyBuilder
---


# Data Visualization Project Summary

{(whoami|}Ollie Keers, ollie.keers.2@city.ac.uk {|whoami)}

{(task|}

You should complete this datavis project summary document and submit it, along with any necessary supplementary files to Moodle by **Sunday 18th April, 5pm UK time (16:00 GMT)**. Submissions will be awarded up to **80 marks** towards your coursework assessment total.

You are also encouraged to regularly commit and push changes to your datavis project throughout the term as you develop your project.

{|task)}

{(questions|}

- Add your question here
- Add your question here
- etc.

{|questions)}

{(visualization|}
```elm {r}
teens : String
teens = "10-17 year-olds"
```
```elm {v interactive}
vaxes1 : Spec
vaxes1 =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration
                    (coAxis
                        [ axcoTicks False
                        , axcoDomain False
                        ,  axcoLabelAngle 0
                        ]
                    )

        data =
            dataFromColumns []
                << dataColumn "Probability of being searched (%)" (nums [ 13.8, 0])
                << dataColumn "b" (nums [0, 0 ])

        enc =
            encoding
                << position X [ pName "b", pOrdinal,  pAxis [] ]
                << position Y [ pName "Probability of being searched (%)", pQuant ]
    in
    toVegaLite [ cfg [], width 1, height 276, data [], enc [], bar [] ]

sankeyAsianT : Spec
sankeyAsianT =
    let
        atdata =
            V.dataFromColumns "Linked Outcomes" []
                << V.dataColumn "Object of search" (V.vStrs (strColumn "Object of search" asianT))
                << V.dataColumn "Outcome" (V.vStrs (strColumn "Outcome" asianT))
                << V.dataColumn "count" (V.vNums (numColumn "count" asianT))

        atsankey =
            { data = atdata []
            , dataSource = "Linked Outcomes"
            , oName = "Object of search"
            , dName = "Outcome"
            , vName = "count"
            , width = 310
            , height = 55
            , oLabel = "Object of search"
            , dLabel = "Outcome"
            , cScale =
                categoricalDomainMap
                    [ ( "Anything to threaten or harm anyone", "rgb(235,213,87)" )
                    , ("Articles for use in criminal damage", "rgb(176,120,0)")
                    , ( "Controlled drugs", "rgb(0,135,73)" )
                    , ( "Evidence of offences under the Act", "rgb(120,0,125)" )
                    , ( "Firearms", "rgb(136,60,0)" )
                    , ( "Fireworks", "rgb(73,6,65)" )
                    , ( "Offensive weapons", "rgb(160,0,51)" )
                    , ( "Stolen goods", "rgb(0,106,205)" )
                    , ( "A no further action disposal", "rgb(255,125,150)" )
                    , ( "Arrest", "rgb(156,218,87)" )
                    , ( "Caution (simple or conditional)", "rgb(208,118,235)" )
                    , ( "Community resolution", "rgb(251,165,56)" )
                    , ( "Penalty Notice for Disorder", "rgb(150,169,95)" )
                    , ( "Summons / charged by post", "rgb(255,172,126)" )
                    ]
            }
    in
    sankeyBuilder atsankey

sankeyBlackT : Spec
sankeyBlackT =
    let
        btdata =
            V.dataFromColumns "Linked Outcomes" []
                << V.dataColumn "Object of search" (V.vStrs (strColumn "Object of search" blackT))
                << V.dataColumn "Outcome" (V.vStrs (strColumn "Outcome" blackT))
                << V.dataColumn "count" (V.vNums (numColumn "count" blackT))

        btsankey =
            { data = btdata []
            , dataSource = "Linked Outcomes"
            , oName = "Object of search"
            , dName = "Outcome"
            , vName = "count"
            , width = 310
            , height = 236
            , oLabel = "Object of search"
            , dLabel = "Outcome"
            , cScale =
                categoricalDomainMap
                    [ ( "Anything to threaten or harm anyone", "rgb(235,213,87)" )
                    , ("Articles for use in criminal damage", "rgb(176,120,0)")
                    , ( "Controlled drugs", "rgb(0,135,73)" )
                    , ( "Evidence of offences under the Act", "rgb(120,0,125)" )
                    , ( "Firearms", "rgb(136,60,0)" )
                    , ( "Fireworks", "rgb(73,6,65)" )
                    , ( "Offensive weapons", "rgb(160,0,51)" )
                    , ( "Stolen goods", "rgb(0,106,205)" )
                    , ( "A no further action disposal", "rgb(255,125,150)" )
                    , ( "Arrest", "rgb(156,218,87)" )
                    , ( "Caution (simple or conditional)", "rgb(208,118,235)" )
                    , ( "Community resolution", "rgb(251,165,56)" )
                    , ( "Penalty Notice for Disorder", "rgb(150,169,95)" )
                    , ( "Summons / charged by post", "rgb(255,172,126)" )
                    ]
            }
    in
    sankeyBuilder btsankey

sankeyOtherT : Spec
sankeyOtherT =
    let
        otdata =
            V.dataFromColumns "Linked Outcomes" []
                << V.dataColumn "Object of search" (V.vStrs (strColumn "Object of search" otherT))
                << V.dataColumn "Outcome" (V.vStrs (strColumn "Outcome" otherT))
                << V.dataColumn "count" (V.vNums (numColumn "count" otherT))

        otsankey =
            { data = otdata []
            , dataSource = "Linked Outcomes"
            , oName = "Object of search"
            , dName = "Outcome"
            , vName = "count"
            , width = 310
            , height = 93
            , oLabel = "Object of search"
            , dLabel = "Outcome"
            , cScale =
                categoricalDomainMap
                    [ ( "Anything to threaten or harm anyone", "rgb(235,213,87)" )
                    , ("Articles for use in criminal damage", "rgb(176,120,0)")
                    , ( "Controlled drugs", "rgb(0,135,73)" )
                    , ( "Evidence of offences under the Act", "rgb(120,0,125)" )
                    , ( "Firearms", "rgb(136,60,0)" )
                    , ( "Fireworks", "rgb(73,6,65)" )
                    , ( "Offensive weapons", "rgb(160,0,51)" )
                    , ( "Stolen goods", "rgb(0,106,205)" )
                    , ( "A no further action disposal", "rgb(255,125,150)" )
                    , ( "Arrest", "rgb(156,218,87)" )
                    , ( "Caution (simple or conditional)", "rgb(208,118,235)" )
                    , ( "Community resolution", "rgb(251,165,56)" )
                    , ( "Penalty Notice for Disorder", "rgb(150,169,95)" )
                    , ( "Summons / charged by post", "rgb(255,172,126)" )
                    ]
            }
    in
    sankeyBuilder otsankey


sankeyWhiteT : Spec
sankeyWhiteT =
    let
        wtdata =
            V.dataFromColumns "Linked Outcomes" []
                << V.dataColumn "Object of search" (V.vStrs (strColumn "Object of search" whiteT))
                << V.dataColumn "Outcome" (V.vStrs (strColumn "Outcome" whiteT))
                << V.dataColumn "count" (V.vNums (numColumn "count" whiteT))

        wtsankey =
            { data = wtdata []
            , dataSource = "Linked Outcomes"
            , oName = "Object of search"
            , dName = "Outcome"
            , vName = "count"
            , width = 310
            , height = 81
            , oLabel = "Object of search"
            , dLabel = "Outcome"
            , cScale =
                categoricalDomainMap
                    [ ( "Anything to threaten or harm anyone", "rgb(235,213,87)" )
                    , ("Articles for use in criminal damage", "rgb(176,120,0)")
                    , ( "Controlled drugs", "rgb(0,135,73)" )
                    , ( "Evidence of offences under the Act", "rgb(120,0,125)" )
                    , ( "Firearms", "rgb(136,60,0)" )
                    , ( "Fireworks", "rgb(73,6,65)" )
                    , ( "Offensive weapons", "rgb(160,0,51)" )
                    , ( "Stolen goods", "rgb(0,106,205)" )
                    , ( "A no further action disposal", "rgb(255,125,150)" )
                    , ( "Arrest", "rgb(156,218,87)" )
                    , ( "Caution (simple or conditional)", "rgb(208,118,235)" )
                    , ( "Community resolution", "rgb(251,165,56)" )
                    , ( "Penalty Notice for Disorder", "rgb(150,169,95)" )
                    , ( "Summons / charged by post", "rgb(255,172,126)" )
                    ]
            }
    in
    sankeyBuilder wtsankey
```
```elm {v}
axes1 : Spec
axes1 =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration
                    (coAxis
                        [ axcoTicks False
                        , axcoDomain False
                        , axcoLabelPadding -12
                        , axcoLabelAngle 0
                        ]
                    )

        data =
            dataFromColumns []
                << dataColumn "Officer-defined ethnicity" (strs [ "Asian", "Black", "Other", "White"])
                << dataColumn "b" (nums [0, 0, 0, 0 ])

        enc =
            encoding
                << position X [ pName "Officer-defined ethnicity", pOrdinal ]
                << position Y [ pName "b", pQuant, pAxis [] ]
    in
    toVegaLite [ cfg [], width 1390, height 1, data [], enc [], bar [] ]
```
```elm {r}
younger : String
younger = "18-24 year-olds"
```
```elm {v interactive}
vaxes2 : Spec
vaxes2 =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration
                    (coAxis
                        [ axcoTicks False
                        , axcoDomain False
                        ,  axcoLabelAngle 0
                        ]
                    )

        data =
            dataFromColumns []
                << dataColumn "Probability of being searched (%)" (nums [ 41.7, 0])
                << dataColumn "b" (nums [0, 0 ])

        enc =
            encoding
                << position X [ pName "b", pOrdinal,  pAxis [] ]
                << position Y [ pName "Probability of being searched (%)", pQuant ]
    in
    toVegaLite [ cfg [], width 1, height 900, data [], enc [], bar [] ]

sankeyAsianY : Spec
sankeyAsianY =
    let
        aydata =
            V.dataFromColumns "Linked Outcomes" []
                << V.dataColumn "Object of search" (V.vStrs (strColumn "Object of search" asianY))
                << V.dataColumn "Outcome" (V.vStrs (strColumn "Outcome" asianY))
                << V.dataColumn "count" (V.vNums (numColumn "count" asianY))

        aysankey =
            { data = aydata []
            , dataSource = "Linked Outcomes"
            , oName = "Object of search"
            , dName = "Outcome"
            , vName = "count"
            , width = 310
            , height = 276
            , oLabel = "Object of search"
            , dLabel = "Outcome"
            , cScale =
                categoricalDomainMap
                    [ ( "Anything to threaten or harm anyone", "rgb(235,213,87)" )
                    , ("Articles for use in criminal damage", "rgb(176,120,0)")
                    , ( "Controlled drugs", "rgb(0,135,73)" )
                    , ( "Evidence of offences under the Act", "rgb(120,0,125)" )
                    , ( "Firearms", "rgb(136,60,0)" )
                    , ( "Fireworks", "rgb(73,6,65)" )
                    , ( "Offensive weapons", "rgb(160,0,51)" )
                    , ( "Stolen goods", "rgb(0,106,205)" )
                    , ( "A no further action disposal", "rgb(255,125,150)" )
                    , ( "Arrest", "rgb(156,218,87)" )
                    , ( "Caution (simple or conditional)", "rgb(208,118,235)" )
                    , ( "Community resolution", "rgb(251,165,56)" )
                    , ( "Penalty Notice for Disorder", "rgb(150,169,95)" )
                    , ( "Summons / charged by post", "rgb(255,172,126)" )
                    ]
            }
    in
    sankeyBuilder aysankey

sankeyBlackY : Spec
sankeyBlackY =
    let
        bydata =
            V.dataFromColumns "Linked Outcomes" []
                << V.dataColumn "Object of search" (V.vStrs (strColumn "Object of search" blackY))
                << V.dataColumn "Outcome" (V.vStrs (strColumn "Outcome" blackY))
                << V.dataColumn "count" (V.vNums (numColumn "count" blackY))

        bysankey =
            { data = bydata []
            , dataSource = "Linked Outcomes"
            , oName = "Object of search"
            , dName = "Outcome"
            , vName = "count"
            , width = 310
            , height = 835
            , oLabel = "Object of search"
            , dLabel = "Outcome"
            , cScale =
                categoricalDomainMap
                    [ ( "Anything to threaten or harm anyone", "rgb(235,213,87)" )
                    , ("Articles for use in criminal damage", "rgb(176,120,0)")
                    , ( "Controlled drugs", "rgb(0,135,73)" )
                    , ( "Evidence of offences under the Act", "rgb(120,0,125)" )
                    , ( "Firearms", "rgb(136,60,0)" )
                    , ( "Fireworks", "rgb(73,6,65)" )
                    , ( "Offensive weapons", "rgb(160,0,51)" )
                    , ( "Stolen goods", "rgb(0,106,205)" )
                    , ( "A no further action disposal", "rgb(255,125,150)" )
                    , ( "Arrest", "rgb(156,218,87)" )
                    , ( "Caution (simple or conditional)", "rgb(208,118,235)" )
                    , ( "Community resolution", "rgb(251,165,56)" )
                    , ( "Penalty Notice for Disorder", "rgb(150,169,95)" )
                    , ( "Summons / charged by post", "rgb(255,172,126)" )
                    ]
            }
    in
    sankeyBuilder bysankey

sankeyOtherY : Spec
sankeyOtherY =
    let
        oydata =
            V.dataFromColumns "Linked Outcomes" []
                << V.dataColumn "Object of search" (V.vStrs (strColumn "Object of search" otherY))
                << V.dataColumn "Outcome" (V.vStrs (strColumn "Outcome" otherY))
                << V.dataColumn "count" (V.vNums (numColumn "count" otherY))

        oysankey =
            { data = oydata []
            , dataSource = "Linked Outcomes"
            , oName = "Object of search"
            , dName = "Outcome"
            , vName = "count"
            , width = 310
            , height = 380
            , oLabel = "Object of search"
            , dLabel = "Outcome"
            , cScale =
                categoricalDomainMap
                    [ ( "Anything to threaten or harm anyone", "rgb(235,213,87)" )
                    , ("Articles for use in criminal damage", "rgb(176,120,0)")
                    , ( "Controlled drugs", "rgb(0,135,73)" )
                    , ( "Evidence of offences under the Act", "rgb(120,0,125)" )
                    , ( "Firearms", "rgb(136,60,0)" )
                    , ( "Fireworks", "rgb(73,6,65)" )
                    , ( "Offensive weapons", "rgb(160,0,51)" )
                    , ( "Stolen goods", "rgb(0,106,205)" )
                    , ( "A no further action disposal", "rgb(255,125,150)" )
                    , ( "Arrest", "rgb(156,218,87)" )
                    , ( "Caution (simple or conditional)", "rgb(208,118,235)" )
                    , ( "Community resolution", "rgb(251,165,56)" )
                    , ( "Penalty Notice for Disorder", "rgb(150,169,95)" )
                    , ( "Summons / charged by post", "rgb(255,172,126)" )
                    ]
            }
    in
    sankeyBuilder oysankey


sankeyWhiteY : Spec
sankeyWhiteY =
    let
        wydata =
            V.dataFromColumns "Linked Outcomes" []
                << V.dataColumn "Object of search" (V.vStrs (strColumn "Object of search" whiteY))
                << V.dataColumn "Outcome" (V.vStrs (strColumn "Outcome" whiteY))
                << V.dataColumn "count" (V.vNums (numColumn "count" whiteY))

        wysankey =
            { data = wydata []
            , dataSource = "Linked Outcomes"
            , oName = "Object of search"
            , dName = "Outcome"
            , vName = "count"
            , width = 310
            , height = 178
            , oLabel = "Object of search"
            , dLabel = "Outcome"
            , cScale =
                categoricalDomainMap
                    [ ( "Anything to threaten or harm anyone", "rgb(235,213,87)" )
                    , ("Articles for use in criminal damage", "rgb(176,120,0)")
                    , ( "Controlled drugs", "rgb(0,135,73)" )
                    , ( "Evidence of offences under the Act", "rgb(120,0,125)" )
                    , ( "Firearms", "rgb(136,60,0)" )
                    , ( "Fireworks", "rgb(73,6,65)" )
                    , ( "Offensive weapons", "rgb(160,0,51)" )
                    , ( "Stolen goods", "rgb(0,106,205)" )
                    , ( "A no further action disposal", "rgb(255,125,150)" )
                    , ( "Arrest", "rgb(156,218,87)" )
                    , ( "Caution (simple or conditional)", "rgb(208,118,235)" )
                    , ( "Community resolution", "rgb(251,165,56)" )
                    , ( "Penalty Notice for Disorder", "rgb(150,169,95)" )
                    , ( "Summons / charged by post", "rgb(255,172,126)" )
                    ]
            }
    in
    sankeyBuilder wysankey
```
```elm {v}
axes2 : Spec
axes2 =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration
                    (coAxis
                        [ axcoTicks False
                        , axcoDomain False
                        , axcoLabelPadding -12
                        , axcoLabelAngle 0
                        ]
                    )

        data =
            dataFromColumns []
                << dataColumn "Officer-defined ethnicity" (strs [ "Asian", "Black", "Other", "White"])
                << dataColumn "b" (nums [0, 0, 0, 0 ])

        enc =
            encoding
                << position X [ pName "Officer-defined ethnicity", pOrdinal ]
                << position Y [ pName "b", pQuant, pAxis [] ]
    in
    toVegaLite [ cfg [], width 1390, height 1, data [], enc [], bar [] ]
```
```elm {r}
mid : String
mid = "25-34 year-olds"
```
```elm {v interactive}
vaxes3 : Spec
vaxes3 =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration
                    (coAxis
                        [ axcoTicks False
                        , axcoDomain False
                        ,  axcoLabelAngle 0
                        ]
                    )

        data =
            dataFromColumns []
                << dataColumn "Probability of being searched (%)" (nums [ 19.0, 0])
                << dataColumn "b" (nums [0, 0 ])

        enc =
            encoding
                << position X [ pName "b", pOrdinal,  pAxis [] ]
                << position Y [ pName "Probability of being searched (%)", pQuant ]
    in
    toVegaLite [ cfg [], width 1, height 400, data [], enc [], bar [] ]

sankeyAsianM : Spec
sankeyAsianM =
    let
        amdata =
            V.dataFromColumns "Linked Outcomes" []
                << V.dataColumn "Object of search" (V.vStrs (strColumn "Object of search" asianM))
                << V.dataColumn "Outcome" (V.vStrs (strColumn "Outcome" asianM))
                << V.dataColumn "count" (V.vNums (numColumn "count" asianM))

        amsankey =
            { data = amdata []
            , dataSource = "Linked Outcomes"
            , oName = "Object of search"
            , dName = "Outcome"
            , vName = "count"
            , width = 310
            , height = 93
            , oLabel = "Object of search"
            , dLabel = "Outcome"
            , cScale =
                categoricalDomainMap
                    [ ( "Anything to threaten or harm anyone", "rgb(235,213,87)" )
                    , ("Articles for use in criminal damage", "rgb(176,120,0)")
                    , ( "Controlled drugs", "rgb(0,135,73)" )
                    , ( "Evidence of offences under the Act", "rgb(120,0,125)" )
                    , ( "Firearms", "rgb(136,60,0)" )
                    , ( "Fireworks", "rgb(73,6,65)" )
                    , ( "Offensive weapons", "rgb(160,0,51)" )
                    , ( "Stolen goods", "rgb(0,106,205)" )
                    , ( "A no further action disposal", "rgb(255,125,150)" )
                    , ( "Arrest", "rgb(156,218,87)" )
                    , ( "Caution (simple or conditional)", "rgb(208,118,235)" )
                    , ( "Community resolution", "rgb(251,165,56)" )
                    , ( "Penalty Notice for Disorder", "rgb(150,169,95)" )
                    , ( "Summons / charged by post", "rgb(255,172,126)" )
                    ]
            }
    in
    sankeyBuilder amsankey

sankeyBlackM : Spec
sankeyBlackM =
    let
        bmdata =
            V.dataFromColumns "Linked Outcomes" []
                << V.dataColumn "Object of search" (V.vStrs (strColumn "Object of search" blackM))
                << V.dataColumn "Outcome" (V.vStrs (strColumn "Outcome" blackM))
                << V.dataColumn "count" (V.vNums (numColumn "count" blackM))

        bmsankey =
            { data = bmdata []
            , dataSource = "Linked Outcomes"
            , oName = "Object of search"
            , dName = "Outcome"
            , vName = "count"
            , width = 310
            , height = 380
            , oLabel = "Object of search"
            , dLabel = "Outcome"
            , cScale =
                categoricalDomainMap
                    [ ( "Anything to threaten or harm anyone", "rgb(235,213,87)" )
                    , ("Articles for use in criminal damage", "rgb(176,120,0)")
                    , ( "Controlled drugs", "rgb(0,135,73)" )
                    , ( "Evidence of offences under the Act", "rgb(120,0,125)" )
                    , ( "Firearms", "rgb(136,60,0)" )
                    , ( "Fireworks", "rgb(73,6,65)" )
                    , ( "Offensive weapons", "rgb(160,0,51)" )
                    , ( "Stolen goods", "rgb(0,106,205)" )
                    , ( "A no further action disposal", "rgb(255,125,150)" )
                    , ( "Arrest", "rgb(156,218,87)" )
                    , ( "Caution (simple or conditional)", "rgb(208,118,235)" )
                    , ( "Community resolution", "rgb(251,165,56)" )
                    , ( "Penalty Notice for Disorder", "rgb(150,169,95)" )
                    , ( "Summons / charged by post", "rgb(255,172,126)" )
                    ]
            }
    in
    sankeyBuilder bmsankey

sankeyOtherM : Spec
sankeyOtherM =
    let
        omdata =
            V.dataFromColumns "Linked Outcomes" []
                << V.dataColumn "Object of search" (V.vStrs (strColumn "Object of search" otherM))
                << V.dataColumn "Outcome" (V.vStrs (strColumn "Outcome" otherM))
                << V.dataColumn "count" (V.vNums (numColumn "count" otherM))

        omsankey =
            { data = omdata []
            , dataSource = "Linked Outcomes"
            , oName = "Object of search"
            , dName = "Outcome"
            , vName = "count"
            , width = 310
            , height = 101
            , oLabel = "Object of search"
            , dLabel = "Outcome"
            , cScale =
                categoricalDomainMap
                    [ ( "Anything to threaten or harm anyone", "rgb(235,213,87)" )
                    , ("Articles for use in criminal damage", "rgb(176,120,0)")
                    , ( "Controlled drugs", "rgb(0,135,73)" )
                    , ( "Evidence of offences under the Act", "rgb(120,0,125)" )
                    , ( "Firearms", "rgb(136,60,0)" )
                    , ( "Fireworks", "rgb(73,6,65)" )
                    , ( "Offensive weapons", "rgb(160,0,51)" )
                    , ( "Stolen goods", "rgb(0,106,205)" )
                    , ( "A no further action disposal", "rgb(255,125,150)" )
                    , ( "Arrest", "rgb(156,218,87)" )
                    , ( "Caution (simple or conditional)", "rgb(208,118,235)" )
                    , ( "Community resolution", "rgb(251,165,56)" )
                    , ( "Penalty Notice for Disorder", "rgb(150,169,95)" )
                    , ( "Summons / charged by post", "rgb(255,172,126)" )
                    ]
            }
    in
    sankeyBuilder omsankey


sankeyWhiteM : Spec
sankeyWhiteM =
    let
        wmdata =
            V.dataFromColumns "Linked Outcomes" []
                << V.dataColumn "Object of search" (V.vStrs (strColumn "Object of search" whiteM))
                << V.dataColumn "Outcome" (V.vStrs (strColumn "Outcome" whiteM))
                << V.dataColumn "count" (V.vNums (numColumn "count" whiteM))

        wmsankey =
            { data = wmdata []
            , dataSource = "Linked Outcomes"
            , oName = "Object of search"
            , dName = "Outcome"
            , vName = "count"
            , width = 310
            , height = 18
            , oLabel = "Object of search"
            , dLabel = "Outcome"
            , cScale =
                categoricalDomainMap
                    [ ( "Anything to threaten or harm anyone", "rgb(235,213,87)" )
                    , ("Articles for use in criminal damage", "rgb(176,120,0)")
                    , ( "Controlled drugs", "rgb(0,135,73)" )
                    , ( "Evidence of offences under the Act", "rgb(120,0,125)" )
                    , ( "Firearms", "rgb(136,60,0)" )
                    , ( "Fireworks", "rgb(73,6,65)" )
                    , ( "Offensive weapons", "rgb(160,0,51)" )
                    , ( "Stolen goods", "rgb(0,106,205)" )
                    , ( "A no further action disposal", "rgb(255,125,150)" )
                    , ( "Arrest", "rgb(156,218,87)" )
                    , ( "Caution (simple or conditional)", "rgb(208,118,235)" )
                    , ( "Community resolution", "rgb(251,165,56)" )
                    , ( "Penalty Notice for Disorder", "rgb(150,169,95)" )
                    , ( "Summons / charged by post", "rgb(255,172,126)" )
                    ]
            }
    in
    sankeyBuilder wmsankey
```
```elm {v}
axes3 : Spec
axes3 =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration
                    (coAxis
                        [ axcoTicks False
                        , axcoDomain False
                        , axcoLabelPadding -12
                        , axcoLabelAngle 0
                        ]
                    )

        data =
            dataFromColumns []
                << dataColumn "Officer-defined ethnicity" (strs [ "Asian", "Black", "Other", "White"])
                << dataColumn "b" (nums [0, 0, 0, 0 ])

        enc =
            encoding
                << position X [ pName "Officer-defined ethnicity", pOrdinal ]
                << position Y [ pName "b", pQuant, pAxis [] ]
    in
    toVegaLite [ cfg [], width 1390, height 1, data [], enc [], bar [] ]
```
```elm {r}
older : String
older = "Over 34 years old"
```
```elm {v interactive}
vaxes4 : Spec
vaxes4 =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration
                    (coAxis
                        [ axcoTicks False
                        , axcoDomain False
                        ,  axcoLabelAngle 0
                        ]
                    )

        data =
            dataFromColumns []
                << dataColumn "Probability of being searched (%)" (nums [ 8.9, 0])
                << dataColumn "b" (nums [0, 0 ])

        enc =
            encoding
                << position X [ pName "b", pOrdinal,  pAxis [] ]
                << position Y [ pName "Probability of being searched (%)", pQuant ]
    in
    toVegaLite [ cfg [], width 1, height 178, data [], enc [], bar [] ]

sankeyAsianO : Spec
sankeyAsianO =
    let
        aodata =
            V.dataFromColumns "Linked Outcomes" []
                << V.dataColumn "Object of search" (V.vStrs (strColumn "Object of search" asianO))
                << V.dataColumn "Outcome" (V.vStrs (strColumn "Outcome" asianO))
                << V.dataColumn "count" (V.vNums (numColumn "count" asianO))

        aosankey =
            { data = aodata []
            , dataSource = "Linked Outcomes"
            , oName = "Object of search"
            , dName = "Outcome"
            , vName = "count"
            , width = 310
            , height = 81
            , oLabel = "Object of search"
            , dLabel = "Outcome"
            , cScale =
                categoricalDomainMap
                    [ ( "Anything to threaten or harm anyone", "rgb(235,213,87)" )
                    , ("Articles for use in criminal damage", "rgb(176,120,0)")
                    , ( "Controlled drugs", "rgb(0,135,73)" )
                    , ( "Evidence of offences under the Act", "rgb(120,0,125)" )
                    , ( "Firearms", "rgb(136,60,0)" )
                    , ( "Fireworks", "rgb(73,6,65)" )
                    , ( "Offensive weapons", "rgb(160,0,51)" )
                    , ( "Stolen goods", "rgb(0,106,205)" )
                    , ( "A no further action disposal", "rgb(255,125,150)" )
                    , ( "Arrest", "rgb(156,218,87)" )
                    , ( "Caution (simple or conditional)", "rgb(208,118,235)" )
                    , ( "Community resolution", "rgb(251,165,56)" )
                    , ( "Penalty Notice for Disorder", "rgb(150,169,95)" )
                    , ( "Summons / charged by post", "rgb(255,172,126)" )
                    ]
            }
    in
    sankeyBuilder aosankey

sankeyBlackO : Spec
sankeyBlackO =
    let
        bodata =
            V.dataFromColumns "Linked Outcomes" []
                << V.dataColumn "Object of search" (V.vStrs (strColumn "Object of search" blackO))
                << V.dataColumn "Outcome" (V.vStrs (strColumn "Outcome" blackO))
                << V.dataColumn "count" (V.vNums (numColumn "count" blackO))

        bosankey =
            { data = bodata []
            , dataSource = "Linked Outcomes"
            , oName = "Object of search"
            , dName = "Outcome"
            , vName = "count"
            , width = 310
            , height = 178
            , oLabel = "Object of search"
            , dLabel = "Outcome"
            , cScale =
                categoricalDomainMap
                    [ ( "Anything to threaten or harm anyone", "rgb(235,213,87)" )
                    , ("Articles for use in criminal damage", "rgb(176,120,0)")
                    , ( "Controlled drugs", "rgb(0,135,73)" )
                    , ( "Evidence of offences under the Act", "rgb(120,0,125)" )
                    , ( "Firearms", "rgb(136,60,0)" )
                    , ( "Fireworks", "rgb(73,6,65)" )
                    , ( "Offensive weapons", "rgb(160,0,51)" )
                    , ( "Stolen goods", "rgb(0,106,205)" )
                    , ( "A no further action disposal", "rgb(255,125,150)" )
                    , ( "Arrest", "rgb(156,218,87)" )
                    , ( "Caution (simple or conditional)", "rgb(208,118,235)" )
                    , ( "Community resolution", "rgb(251,165,56)" )
                    , ( "Penalty Notice for Disorder", "rgb(150,169,95)" )
                    , ( "Summons / charged by post", "rgb(255,172,126)" )
                    ]
            }
    in
    sankeyBuilder bosankey

sankeyOtherO : Spec
sankeyOtherO =
    let
        oodata =
            V.dataFromColumns "Linked Outcomes" []
                << V.dataColumn "Object of search" (V.vStrs (strColumn "Object of search" otherO))
                << V.dataColumn "Outcome" (V.vStrs (strColumn "Outcome" otherO))
                << V.dataColumn "count" (V.vNums (numColumn "count" otherO))

        oosankey =
            { data = oodata []
            , dataSource = "Linked Outcomes"
            , oName = "Object of search"
            , dName = "Outcome"
            , vName = "count"
            , width = 310
            , height = 63
            , oLabel = "Object of search"
            , dLabel = "Outcome"
            , cScale =
                categoricalDomainMap
                    [ ( "Anything to threaten or harm anyone", "rgb(235,213,87)" )
                    , ("Articles for use in criminal damage", "rgb(176,120,0)")
                    , ( "Controlled drugs", "rgb(0,135,73)" )
                    , ( "Evidence of offences under the Act", "rgb(120,0,125)" )
                    , ( "Firearms", "rgb(136,60,0)" )
                    , ( "Fireworks", "rgb(73,6,65)" )
                    , ( "Offensive weapons", "rgb(160,0,51)" )
                    , ( "Stolen goods", "rgb(0,106,205)" )
                    , ( "A no further action disposal", "rgb(255,125,150)" )
                    , ( "Arrest", "rgb(156,218,87)" )
                    , ( "Caution (simple or conditional)", "rgb(208,118,235)" )
                    , ( "Community resolution", "rgb(251,165,56)" )
                    , ( "Penalty Notice for Disorder", "rgb(150,169,95)" )
                    , ( "Summons / charged by post", "rgb(255,172,126)" )
                    ]
            }
    in
    sankeyBuilder oosankey


sankeyWhiteO : Spec
sankeyWhiteO =
    let
        wodata =
            V.dataFromColumns "Linked Outcomes" []
                << V.dataColumn "Object of search" (V.vStrs (strColumn "Object of search" whiteO))
                << V.dataColumn "Outcome" (V.vStrs (strColumn "Outcome" whiteO))
                << V.dataColumn "count" (V.vNums (numColumn "count" whiteO))

        wosankey =
            { data = wodata []
            , dataSource = "Linked Outcomes"
            , oName = "Object of search"
            , dName = "Outcome"
            , vName = "count"
            , width = 310
            , height = 20
            , oLabel = "Object of search"
            , dLabel = "Outcome"
            , cScale =
                categoricalDomainMap
                    [ ( "Anything to threaten or harm anyone", "rgb(235,213,87)" )
                    , ("Articles for use in criminal damage", "rgb(176,120,0)")
                    , ( "Controlled drugs", "rgb(0,135,73)" )
                    , ( "Evidence of offences under the Act", "rgb(120,0,125)" )
                    , ( "Firearms", "rgb(136,60,0)" )
                    , ( "Fireworks", "rgb(73,6,65)" )
                    , ( "Offensive weapons", "rgb(160,0,51)" )
                    , ( "Stolen goods", "rgb(0,106,205)" )
                    , ( "A no further action disposal", "rgb(255,125,150)" )
                    , ( "Arrest", "rgb(156,218,87)" )
                    , ( "Caution (simple or conditional)", "rgb(208,118,235)" )
                    , ( "Community resolution", "rgb(251,165,56)" )
                    , ( "Penalty Notice for Disorder", "rgb(150,169,95)" )
                    , ( "Summons / charged by post", "rgb(255,172,126)" )
                    ]
            }
    in
    sankeyBuilder wosankey
```
```elm {v}
axes4 : Spec
axes4 =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration
                    (coAxis
                        [ axcoTicks False
                        , axcoDomain False
                        , axcoLabelPadding -12
                        , axcoLabelAngle 0
                        ]
                    )

        data =
            dataFromColumns []
                << dataColumn "Officer-defined ethnicity" (strs [ "Asian", "Black", "Other", "White"])
                << dataColumn "b" (nums [0, 0, 0, 0 ])

        enc =
            encoding
                << position X [ pName "Officer-defined ethnicity", pOrdinal ]
                << position Y [ pName "b", pQuant, pAxis [] ]
    in
    toVegaLite [ cfg [], width 1390, height 1, data [], enc [], bar [] ]
```
{|visualization)}

{(insights|}

1.  Insight one
2.  Insight two
3.  Insight three

{|insights)}

{(designJustification|}

1.  Choice one
2.  Choice two
3.  Choice three

{|designJustification)}

{(validation|}

1.  Evaluation one
2.  Evaluation two
3.  Evaluation three

{|validation)}

{(references|}

{|references)}
