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

- What disparities exist in the Metropolitan Police Service's appplication of Stop & Search to different groups?
- How do individual experiences of Stop & Search differ by ethnicity?
- Does the application of Stop & Search differ depending on the suspected offence?

{|questions)}

{(visualization|}

```elm {v interactive}
expectedSearches : Spec
expectedSearches =
    let
        data =  dataFromColumns []
                << dataColumn "Age" (numColumn "Age" cumulativeSearches |> nums)
                << dataColumn "Searches" (numColumn "Searches" cumulativeSearches |> nums)
                << dataColumn "Officer-Defined Ethnicity" (strColumn "Officer-Defined Ethnicity" cumulativeSearches |> strs)


        enc =
            encoding
                << position X [ pName "Age", pQuant, pTitle "Age" ]

        transSelFilter =
            transform
                << filter (fiSelection "hover")
        enc1 =
            encoding
                << position Y [ pName "Searches", pQuant, pTitle "Expected Cumulative Lifetime  Searches" ]
                << color [ mName "Officer-Defined Ethnicity", mTitle "Officer-defined ethnicity", mScale[ scScheme "dark2" [] ]]

        spec1 =
            asSpec
                [ enc1 []
                , layer
                    [ asSpec [ line [] ]
                    , asSpec [ transSelFilter [], point [] ]
                    ]
                ]

        sel =
            selection
                << select "hover"
                    seSingle
                    [ seFields [ "Age" ]
                    , seEmpty
                    , seOn "mouseover"
                    , seClear "mouseout"
                    , seNearest True
                    ]

        transPivot =
            transform
                << pivot "Officer-Defined Ethnicity" "Searches" [ piGroupBy [ "Age" ] ]

        enc2 =
            encoding
                << opacity [ mSelectionCondition (expr "hover") [ mNum 0.3 ] [ mNum 0 ] ]
                << tooltips [ [ tName "Asian" ], [ tName "Black" ], [ tName "Other" ], [ tName "White" ] ]

        spec2 =
            asSpec [ sel [], transPivot [], enc2 [], rule [] ]

    in
    toVegaLite [ width 1250, height 400, data [], enc [], layer [spec1, spec2] ]
```

```elm {l=hidden}
vaxes0 : Spec
vaxes0 =
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
                << dataColumn "Searches in 2020" (nums [ 0, 0])
                << dataColumn "b" (nums [0, 0 ])

        enc =
            encoding
                << position X [ pName "b", pOrdinal,  pAxis [] ]
                << position Y [ pName "Searches in 2020", pQuant, pAxis [] ]
    in
    toVegaLite [ cfg [], width 15, height 1, data [], enc [], bar [] ]
histogramA : Spec
histogramA =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration
                    (coAxis
                        [ axcoTicks False
                        , axcoDomain False

                        , axcoLabelAngle 0
                        ]
                    )
        data =
            dataFromColumns []
                << dataColumn "Age Range" ( strs [ "10-17", "18-24", "25-34", "over 34"])
                << dataColumn "chance" (nums [2.7, 13.8, 3.8, 0.7 ])
        enc =
            encoding
                << position X
                    [ pName "Age Range"
                    , pNominal

                    , pScale [ scPaddingInner 0.05 ]
                    , pAxis [ axTitle "Asian" ]
                    ]
                << position Y
                    [ pName "chance", pQuant
                    , pAxis [ axTitle "Probability of being searched in 2020 (%)"]
                    , pScale [ scDomain (doNums [ 0, 42 ])]
                    ]

    in
    toVegaLite
        [ width 77.5
        , height (400)
        , cfg []
        , data []
        , enc []
        , bar []
        ]

histogramB : Spec
histogramB =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration
                    (coAxis
                        [ axcoTicks False
                        , axcoDomain False

                        , axcoLabelAngle 0
                        ]
                    )
        data =
            dataFromColumns []
                << dataColumn "Age Range" ( strs [ "10-17", "18-24", "25-34", "over 34"])
                << dataColumn "chance" (nums [11.8, 41.7, 14.7, 2.5 ])
        enc =
            encoding
                << position X
                    [ pName "Age Range"
                    , pNominal

                    , pScale [ scPaddingInner 0.05 ]
                    , pAxis [ axTitle "Black" ]
                    ]
                << position Y
                    [ pName "chance", pQuant
                    , pAxis [ axTitle ""]
                    , pScale [ scDomain (doNums [ 0, 42 ])]
                    ]

    in
    toVegaLite
        [ width 77.5
        , height (400)
        , cfg []
        , data []
        , enc []
        , bar []
        ]

histogramO : Spec
histogramO =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration
                    (coAxis
                        [ axcoTicks False
                        , axcoDomain False

                        , axcoLabelAngle 0
                        ]
                    )
        data =
            dataFromColumns []
                << dataColumn "Age Range" ( strs [ "10-17", "18-24", "25-34", "over 34"])
                << dataColumn "chance" (nums [4.7, 19.0, 5.1, 0.9 ])
        enc =
            encoding
                << position X
                    [ pName "Age Range"
                    , pNominal

                    , pScale [ scPaddingInner 0.05 ]
                    , pAxis [ axTitle "Other" ]
                    ]
                << position Y
                    [ pName "chance", pQuant
                    , pAxis [ axTitle ""]
                    , pScale [ scDomain (doNums [ 0, 42 ])]
                    ]

    in
    toVegaLite
        [ width 77.5
        , height (400)
        , cfg []
        , data []
        , enc []
        , bar []
        ]

histogramW : Spec
histogramW =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration
                    (coAxis
                        [ axcoTicks False
                        , axcoDomain False

                        , axcoLabelAngle 0
                        ]
                    )
        data =
            dataFromColumns []
                << dataColumn "Age Range" ( strs [ "10-17", "18-24", "25-34", "over 34"])
                << dataColumn "chance" (nums [4.0, 8.9, 3.2, 1.0 ])
        enc =
            encoding
                << position X
                    [ pName "Age Range"
                    , pNominal

                    , pScale [ scPaddingInner 0.05 ]
                    , pAxis [ axTitle "White" ]
                    ]
                << position Y
                    [ pName "chance", pQuant
                    , pAxis [ axTitle ""]
                    , pScale [ scDomain (doNums [ 0, 42 ])]
                    ]

    in
    toVegaLite
        [ width 77.5
        , height (400)
        , cfg []
        , data []
        , enc []
        , bar []
        ]
```

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
                << dataColumn "Searches in 2020" (nums [ 21554, 0])
                << dataColumn "b" (nums [0, 0 ])

        enc =
            encoding
                << position X [ pName "b", pOrdinal,  pAxis [] ]
                << position Y [ pName "Searches in 2020", pQuant ]
    in
    toVegaLite [ cfg [], width 1, height 431, data [], enc [], bar [] ]

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
            , height = 125
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
            , height = 431
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
            , height = 43
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
            , height = 382
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
                << dataColumn "Searches in 2020" (nums [ 41788, 0])
                << dataColumn "b" (nums [0, 0 ])

        enc =
            encoding
                << position X [ pName "b", pOrdinal,  pAxis [] ]
                << position Y [ pName "Searches in 2020", pQuant ]
    in
    toVegaLite [ cfg [], width 1, height 886, data [], enc [], bar [] ]

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
            , height = 479
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
            , height = 836
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
            , height = 124
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
            , height = 728
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
                << dataColumn "Searches in 2020" (nums [ 29642, 0])
                << dataColumn "b" (nums [0, 0 ])

        enc =
            encoding
                << position X [ pName "b", pOrdinal,  pAxis [] ]
                << position Y [ pName "Searches in 2020", pQuant ]
    in
    toVegaLite [ cfg [], width 1, height 593, data [], enc [], bar [] ]

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
            , height = 268
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
            , height = 458
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
            , height = 64
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
            , height = 593
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
                << dataColumn "Searches in 2020" (nums [ 27555, 0])
                << dataColumn "b" (nums [0, 0 ])

        enc =
            encoding
                << position X [ pName "b", pOrdinal,  pAxis [] ]
                << position Y [ pName "Searches in 2020", pQuant ]
    in
    toVegaLite [ cfg [], width 1, height 551, data [], enc [], bar [] ]

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
            , height = 128
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
            , height = 296
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
            , height = 30
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
            , height = 551
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

1.	Throughout their lives, people that police officers define as black are far more likely to be stopped & searched than any other ethnicity. This disparity widens throughout their lives. A typical 22-year-old black person will have been searched more times than a person of any other ethnicity will be in their lifetime. For all groups and objects of search, it is most likely that nothing will be found. Stop and search is least successful at finding prohibited articles when deployed in teenagers, where fewer than 16% of searches result in an object being found. For most ethnic groups, at least half of these unsuccessful searches of teenagers were for controlled drugs. This was not the case for black teenagers, who are the subjects of the majority of weapons searches of this age group. When searching for weapons, more unsuccessful searches of black teenagers are conducted than the total number of searches, of all outcomes, for teenagers of all other ethnicities combined. These findings in combination suggest a disproportionate and discriminatory approach to searching young black people, which has the potential to cause persistent damage to their attitudes towards the police and more likely to commit crimes in future[1]. 
2.	For all ethnicities, people aged 18-24 are the most likely to be searched. A community resolution is issued more frequently to 18–24-year-old white people than any other ethnic group. This outcome represents a prohibited article being discovered, but the officer deciding that no formal action should be taken. This suggests that officers may be disproportionately lenient towards young white people. For all ethnicities, searches of the two older age groups, 25-34 and over 34, more frequently led to arrests than for the younger group. This corresponds with a decline in unsuccessful searches, suggesting that searches of older people are more targeted and used more speculatively with younger people. 
3.	More searches are conducted to discover drugs than for any other purpose, with stolen goods and offensive weapons being the other most notable objects of search. For searches that do discover a prohibited article, the police-issued outcome differs depending on the nature of that article. When controlled drugs are found, police are more likely to pursue a community resolution or issue a penalty notice. In successful searches for other items such as stolen goods or offensive weapons, arrest is a more likely outcome. These findings may illustrate a difference in the priorities of search. Widespread searches for drugs with comparatively few arrests suggests that the police prioritise the discovery of drugs over enforcing subsequent consequences. Searches for stolen goods and weapons are less frequent but more likely to lead to arrest, indicating that that enforcement is more of a priority for these offenses.


{|insights)}

{(designJustification|}

1.	**Data feminist & humanist approaches**: D’Ignazio’s work on data feminism discusses the importance of overcounting in data collection, using the example of stop-and-frisk in Boston.[2] This mirrors the pattern of stop and search seen in the 2020 Metropolitan Police data, which is not deployed evenly across ethnicities. This work attempts to explicitly visualise the disparities in the deployment of search in order to convey the police’s responsibility for the issue, rather than the people who are searched. The officer makes the choice of who to search, not the subject, and so “Officer-defined ethnicity” was used throughout, rather than “Self-defined ethnicity”. This further emphasises the power dynamics of the search – the subject’s agency and identity removed by the searching officer, and when transformed into a search statistic. Taking inspiration from Lupi’s data humanist approach [3], the line chart at the top of the visualisation has been designed to show the individual impact of search disparities. Their impact is cumulative, but this is hidden in visualisations that only consider counts or rates of searches. Rather than viewing the data from the usual perspective of the police, this visualisation displays the different lifetime experiences of individuals if the 2020 rates persisted throughout their lives. This portrays police as the perpetrators of search on individuals, inverting the traditional narrative of police responding to crime.
2.	**Tiling multiple scaled visualisations**: This work is intended to promote comparisons, and so viewing multiple visualisations is necessary. Displaying these simultaneously, rather than one at a time, promotes those comparisons and reduces cognitive load for the user[4]. A matrix alignment [5] was therefore used. Each Sankey diagram needed to be large: vertically for the information contained within it to be extracted and horizontally to avoid search descriptor text overlapping. Removal of this text produced visualisations that were less intuitive. These factors led to the production of a very large visualisation. While it is possible to view all four columns on screen at one time, it is only possible to view one row at a time. This makes comparisons between columns easier for the viewer to perform. Rows of consistent age and columns of consistent ethnicity were therefore chosen, in order to prioritise comparisons of the latter. Each of the tiled diagrams has been scaled to reflect the number of searches that have taken place. This scaling has had two main benefits. Firstly, it enables the overall tiled visualisation to be treated as four vertically concatenated histograms. This allows for macroscale comparisons of searches conducted on each of the 16 groups. Secondly, it allows for a consistent scale for the components of each subplot, meaning that comparisons between search details can be made across groups both in absolute numbers, and as a proportion of their searches.
3.	**Use of interactive Sankey Diagrams**: Kosara, Bendix & Hauser introduced parallel sets[6], or Sankey diagrams, for interactive visualisation of multidimensional categorical data.  The data used here consists of multiple categorical descriptors, hence the decision to use them within this work. This allows for visualisation of not only the distribution of the categories, but also the relationships between them. In this case there is an element of conditional probability to the data. Sankey diagrams capture this in a way that a dashboard of separate cross-filtered histograms would not. Interaction with the Sankey diagrams supports lay-users in performing Bayesian reasoning, shown to be challenging with the support of traditional static visualisations[7]. Through interaction it is simple and intuitive to discover the probability that a suspect was searched for drugs given that they were arrested, or that a suspect was arrested given that they were searched for drugs. The sequential nature of the stop and search process was influential in the design.  A person with particular characteristics is seen by a police officer, who then chooses whether to search them which results in an outcome. Placing ‘outcome’ on the right hand stack means that the viewer follows the same journey as the searching officer: a demographic group is chosen, objects of search are considered, followed by outcomes.



{|designJustification)}

{(validation|}

1.	**The design of this visualisation supports the information seeking process of the user, making it effective for this purpose.** Schneiderman’s[8] Visual Information Seeking Mantra provides a framework for evaluating whether the design of a visualisation facilitates the goals of the end user. An effective visualisation would support each stage of this process in order. For this visualisation, the viewer initially sees the line graph which provides an overview of lifetime trends for all groups, which they are able to interact with to provide details on demand. Moving on to the tiled Sankey diagrams, the viewer is initially presented with histograms, providing an overview of general trends, although as they aren’t all simultaneously viewable this visualisation is not successful in providing a complete overview. The viewer is able to filter out age ranges through scrolling and get further details on mouseover. A second iteration of this process is then possible – the viewer has an overview of the distribution for this subset of the data, is able to zoom and filter on particular objects and outcomes of search, before getting details on demand about their interrelationships. The juxtapositioning of many small multiples supports the viewer in relating their insights. 
2.	**Using 'count of searches' as the vertical scale has diminished the aesthetic impact of this work.** A challenge when designing this visualisation was deciding whether to use absolute numbers of searches or search rates per capita for the scaling of the Sankey Diagrams. When using rates, the Sankey diagram for 18-24-year-old black people is over twice the height of any other column at over 800 pixels and dominates the visualisation, which is an important and upsetting finding. However, this scaling causes other columns to be very small – the diagram for over-34-year-old white people would be 20 pixels high, which makes extracting information on the nature of searches impossible. It would also have been necessary to scale the width of the diagrams to represent the populations each represented, creating an even wider visualisation. Absolute numbers were used in the final work to allow for search details to be extracted, at the expense of overall visual impact. The line chart at the top of the work was created to compensate for the absence of per-capita scaling. Ideally, the visualisation would offer the ability to toggle all diagrams between the two scales, as in the excellent Covid visualisations of the Financial Times[9]. I was not able to implement this to work over the multiple specifications and code-blocks required for the concatenation of Vega-coded Sankey Diagrams.

3.	**The ‘tiled histogram’ of Sankey Diagrams is mostly effective at representing aggregated information.** Elmqvist & Fekete’s work on Hierarchical Aggregation for Information Visualizations [10] contains design guidelines for implementing such interactive, multiscale visualisations. While Entity Budget, Visual Summary, Discriminability and Interpretability were adhered to throughout, this was not possible for Visual Simplicity and Fidelity. The principle of visual simplicity was in opposition to the need to extract detail from the visualisation. For the intended purpose, utility was deemed more important than simplicity. This is evident in the complexity of the visualisation. Fidelity becomes an issue during interaction, where selecting a particular object or outcome rescales that component to the full height of the diagram. This appears to suggest that all searches for that group correspond to this component. When a component is selected, comparisons between groups also become more challenging in terms of raw figures, although simpler in terms of proportions. This was a critical issue when scaling by searches per person, as the aggregated information became entirely inconsistent.

{|validation)}

{(references|}
[1]K. Murray, S. McVie, D. Farren, L. Herlitz, M. Hough, and P. Norris, ‘Procedural justice, compliance with the law and police stop-and-search: a study of young people in England and Scotland’, Polic. Soc., pp. 1–20, Jan. 2020, doi: 10.1080/10439463.2020.1711756.
[2]	C. D’Ignazio, ‘15. Data Journalism: What’s Feminism Got to Do With IT?’, Crit. Data Pract., p. 103, 2021.
[3]	G. Lupi, ‘VIS Capstone Address Data Humanism: The Revolution will be Visualized’, in 2017 IEEE Conference on Visual Analytics Science and Technology (VAST), Phoenix, AZ, Oct. 2017, pp. 1–1, doi: 10.1109/VAST.2017.8585625.
[4] M. D. Plumlee and C. Ware, ‘Zooming versus multiple window interfaces: Cognitive costs of visual comparisons’, ACM Trans. Comput.-Hum. Interact., vol. 13, no. 2, pp. 179–209, Jun. 2006, doi: 10.1145/1165734.1165736.
[5]	T. Munzner, Visualization analysis & design. CRC Press, 2014.
[6]	R. Kosara, F. Bendix, and H. Hauser, ‘Parallel Sets: interactive exploration and visual analysis of categorical data’, IEEE Trans. Vis. Comput. Graph., vol. 12, no. 4, pp. 558–568, Jul. 2006, doi: 10.1109/TVCG.2006.76.
[7]	M. Reani, A. Davies, N. Peek, and C. Jay, ‘How do people use information presentation to make decisions in Bayesian reasoning tasks?’, Int. J. Hum.-Comput. Stud., vol. 111, pp. 62–77, Mar. 2018, doi: 10.1016/j.ijhcs.2017.11.004.
[8]	B. Shneiderman, ‘The Eyes Have It: A Task by Data Type Taxonomy for Information Visualizations’, in Proceedings of the 1996 IEEE Symposium on Visual Languages, USA, 1996, p. 336.
[9]https://ig.ft.com/coronavirus-chart/?areas=eur&areas=usa&areas=bra&areas=gbr&areas=cze&areas=hun&areasRegional=usny&areasRegional=usnj&areasRegional=usaz&areasRegional=usca&areasRegional=usnd&areasRegional=ussd&cumulative=0&logScale=0&per100K=1&startDate=2020-09-01&values=deaths
[10] N. Elmqvist and J.-D. Fekete, ‘Hierarchical Aggregation for Information Visualization: Overview, Techniques, and Design Guidelines’, IEEE Trans. Vis. Comput. Graph., vol. 16, no. 3, pp. 439–454, May 2010, doi: 10.1109/TVCG.2009.84.
**Colour Scheme:**
Categorical colourscheme generated using 'I Want Hue": https://medialab.github.io/iwanthue/
**Data:**
Stop and search data acquired from https://data.police.uk under the Open Government Licence version 3.0 (https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/). Population data acquired from projections on the London Datastore (https://data.london.gov.uk/dataset/ethnic-group-population-projections).
Preprocessing steps in python are shown in the accompanying jupyter notebook on GitHub, before manual entry of tidy data into litvis.
{|references)}
