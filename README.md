# pldate
    Simple date-manipulation in Perl (to be used in shell scripts)
    
    Might be useful on platforms (like AIX) that don't have GNU-dateutils.
    
    It only handles dates, not times or timestamps.
    
    It's only supported date-format is YYYYMMDD (but it has a 'printf' command)
    Its internal date-representation is number of days since 1601-01-01,
    supported range: 0..292192 (1601-01-01..2400-12-31).

    Usage:
      pldate                     # just print the current date (localtime)
      pldate [command-list]      # execute the commands and print the result
    
    Commands:
      today                      # set the internal variable to the current day
                                 # (automatically performed at start)
      tomorrow                   # set the internal variable to the next day
      yesterday                  # set the internal variable to the previous day

      set YYYYMMDD               # go to the specifed day
      set-int N                  # the same with internal format (see above)

      add-days N                 # add N days (it can be negative too)
      sub-days N                 # subtract N days (it can be negative too)

      next-wday N                # N=0..7: go to the next Nth day of week
      next-dow  N                # synonym of the previous

      prev-wday N                # N=0..7: go to the previous Nth day of week
      prev-dow  N                # synonym of the previous

      upto-wday N                # N=0..7: go to the next Nth day of week
                                 # but stay, if it is today
                                 # _not_ equivalent with 'next-wday'
      upto-dow  N                # synonym of the previous

      downto-wday N              # N=0..7: go to the previous Nth day of week
                                 # but stay, if it is today
      downto-dow  N              # synonym of the previous

      set-mday N                 # N=1..31: go to Nth day of the month (or the last day)
                                 # N=0: same as N=1
                                 # N<0: go to the abs(N)th day of the month, counting backwards from the end

      set-yday N                 # N=1..366: go to Nth day of the year (or the last day)
                                 # N=0: same as N=1
                                 # N<0: go to the abs(N)th day of the year, counting backwards from the end

      print                      # print the current value as %Y%m%d
      printf FMT                 # formatted print (use %Y,%y,%m,%d,%w,%j and %I for internal number)
    
    Complete examples:
      Next Saturday:
        ./pldate today next-dow 6
    
      Today if it is Saturday, otherways the first Saturday after today:
        ./pldate yesterday next-dow 6
        ./pldate today upto-dow 6
    
      Previous Saturday:
        ./pldate today prev-dow 6
    
      Today if it is Saturday, otherways the previous Saturday before today:
        ./pldate tomorrow prev-dow 6
        ./pldate today downto-dow 6

      Last day of the previous month
        ./pldate today set-mday 1 sub-days 1
    
      First day of the next month
        ./pldate today set-mday -1 add-days 1
    
      The week containing the current date (eg 19681230-19690105)
        ./pldate today \
              add-days 1 \
              prev-dow 1 \
              printf %Y%m%d- \
              next-dow 7 printf %Y%m%d

      Days since a fixed day:
         expr "$(./pldate today printf %I)" - "$(./pldate set 20010209 printf %I)"
