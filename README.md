let
    UTCValue = DateTime.From([DATETIMESTAMP_UTC]),
    CurrentYear = Date.Year(Date.From(UTCValue)),

    MarchFirst = #date(CurrentYear, 3, 1),
    MarchFirstSunday =
        Date.AddDays(
            MarchFirst,
            Number.Mod(7 - Date.DayOfWeek(MarchFirst, Day.Sunday), 7)
        ),
    SecondSundayMarch = Date.AddDays(MarchFirstSunday, 7),

    DSTStartUTC =
        #datetime(
            CurrentYear,
            3,
            Date.Day(SecondSundayMarch),
            8, 0, 0
        ),

    NovemberFirst = #date(CurrentYear, 11, 1),
    FirstSundayNovember =
        Date.AddDays(
            NovemberFirst,
            Number.Mod(7 - Date.DayOfWeek(NovemberFirst, Day.Sunday), 7)
        ),

    DSTEndUTC =
        #datetime(
            CurrentYear,
            11,
            Date.Day(FirstSundayNovember),
            7, 0, 0
        ),

    UTCOffset =
        if UTCValue >= DSTStartUTC and UTCValue < DSTEndUTC
        then -5
        else -6,

    CentralTime =
        UTCValue + #duration(0, UTCOffset, 0, 0)
in
    CentralTime
