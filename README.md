let
    // Define the start and end dates
    StartDate = #date(1996, 1, 1),
    // Set how many years into the future the rolling calendar will go...  (i.e. 1 = one year, 2 = two year, 0=current year, etc.)
    SetFutureYears = 1,
    EndDate = Date.AddYears(Date.From(DateTime.LocalNow()),SetFutureYears),

    // Generate a list of dates from StartDate to EndDate
    DateList = List.Dates(StartDate, Duration.Days(EndDate - StartDate) + 1, #duration(1, 0, 0, 0)),

    // Convert the list into a table
    RollingCalendar = Table.FromList(DateList, Splitter.SplitByNothing(), {"Date"}),
    #"Inserted Year" = Table.AddColumn(RollingCalendar, "Year", each Date.Year([Date]), Int64.Type),
    #"Inserted Month" = Table.AddColumn(#"Inserted Year", "Month", each Date.Month([Date]), Int64.Type),
    #"Inserted Month Name" = Table.AddColumn(#"Inserted Month", "Month Name", each Date.MonthName([Date]), type text),
    #"Inserted Quarter" = Table.AddColumn(#"Inserted Month Name", "Quarter", each Date.QuarterOfYear([Date]), Int64.Type),
    #"Inserted Week of Year" = Table.AddColumn(#"Inserted Quarter", "Week of Year", each Date.WeekOfYear([Date]), Int64.Type)
in
    #"Inserted Week of Year"
