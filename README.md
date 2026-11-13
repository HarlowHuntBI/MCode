let
    UTC = DateTime.From([DATETIMESTAMP_UTC]),
    Year = Date.Year(Date.From(UTC)),

    // Second Sunday in March
    March1 = #date(Year, 3, 1),
    DSTStartDay =
        1 + Number.Mod(
            7 - Date.DayOfWeek(March1, Day.Sunday),
            7
        ) + 7,
    DSTStartUTC =
        #datetime(Year, 3, DSTStartDay, 8, 0, 0),

    // First Sunday in November
    Nov1 = #date(Year, 11, 1),
    DSTEndDay =
        1 + Number.Mod(
            7 - Date.DayOfWeek(Nov1, Day.Sunday),
            7
        ),
    DSTEndUTC =
        #datetime(Year, 11, DSTEndDay, 7, 0, 0),

    Offset =
        if UTC >= DSTStartUTC and UTC < DSTEndUTC
        then -5
        else -6
in
    UTC + #duration(0, Offset, 0, 0)
