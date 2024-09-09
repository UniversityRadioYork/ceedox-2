# Legal Obligations

These, unfortunately, are **not** negotiable, and we need to do them.

## Stop! You Violated The Law

Obviously, don't do anything that's outright, obviously illegal. URY could get in serious trouble, and none of us want that to happen. You could get in serious trouble, and we hate to lose a team member.

Some examples of what **not** to do:

- Store publicly viewable copyrighted material on URY servers

- Put illegal content like child pornography on URY servers, even if it isn't publicly viewable

- Crack into someone else's computer systems from a URY computer

- Put publicly viewable blatantly libellous/defamatory content on URY servers

- Store more personal information than is really necessary on URY computers

## Ofcom

Ofcom requires us to keep a 42-days audiolog of the station's output. This means that there must **always** be at least one 42-day logger running.

## PPL Licence

From time to time you'll need to get figures to back up our PPL licencing requirements.

These run over four quarters:

1.  1st January to 1st April
2.  1st April to 1st July
3.  1st July to 1st October
4.  1st October to 1st January

The first date is inclusive (date >= this) and the second date is exclusive (date < this).

### TODO

Make a nice frontend for doing this.

### Webcast and territory report

#### The New Fancy Way

One of the quarterly requirements is a quarterly webcast and territory report. This is a single spreadsheet and combines

- Track listener hours (Number of tracks played per hour we are broadcasting)

- Percentage of listeners from each country listed

- Total hours of listening

The data for the territory and hours is stored into the database by Listenstats (the proxy sitting between nginx and Icecast).

Run this query in psql on urysteve in membership database to calculate the total listen hours (substituting the relevant dates on the fourth line):

    WITH valid_rows AS (
        SELECT *
        FROM listens.listen
        WHERE (time_start>= '2020-10-01' AND time_end <= '2020-12-06')
       AND time_end - time_start> '30 seconds'
        AND time_end - time_start <'24 hours'
    )
    SELECT EXTRACT(EPOCH FROM SUM(time_end - time_start))/3600 AS hours
    FROM valid_rows;

And run this query to calculate listeners by territory, again substituting dates:

    WITH valid_rows AS (
        SELECT *
        FROM listens.listen
        WHERE (time_start>= '2020-10-01' AND time_end <= '2020-12-06')
       AND time_end - time_start> '30 seconds'
        AND time_end - time_start <'24 hours'
    ), countries as (
        SELECT geoip_country AS country
        FROM valid_rows
        WHERE geoip_country `<>` ''
        AND (NOT ip_address  <<inet '10.240.0.0/16')
        UNION ALL
        SELECT 'GB' AS country
        FROM valid_rows
        WHERE ip_address  <<inet '10.240.0.0/16'
    )
    SELECT country, count(country), count(country)::decimal / (select count(*) from countries) * 100 as prop
    from countries
    group by country
    order by country;

**Note: we make the assumption that anyone listening for less than 30 seconds, or more than 24 hours, is either channel-hopping without actually listening, or a bot scraping/relaying us, and thus discount them.**

These values then need transferring to the PPL spreadsheet (ask management to forward it from the quarterly request email). For each country in the PPL sheet, copy the corresponding value from the processed log data. Enter the total hours into the appropriate field. Check that the percentages tally to 100% to ensure that all the listener figures have been accounted for.

Finally, we need 'total tracks per broadcast hour'. First the easy bit – broadcast hour. A broadcast hour is when we are actually broadcasting music, so term time. These **usually** conveniently fall within the respective quarters (Autumn usually straddles Q3 and Q4), but not always, so check the term dates and any 'special events' which were actually broadcast online. So, assuming a single term fits wholly within a quarter, 7 days _ 10 weeks _ 24 hours = 1680 hours.

More tricky – the number of tracks played. If a full tracklist is requested, care must be taken to make sure the same data is used for both, and it is of course good practice to ensure this anyway. Use the MyRadio "full tracklist" option in the [BAPS Statistics](https///ury.org.uk/myradio/Stats/fullTracklist/) page to generate all the tracks played in a quarter (note – the date ranges are a pain in the arse. The best way is to use an unambiguous date range which can't be misinterpreted as either MMDDYY or DDMMYY). Download as a CSV (it should offer this option) and count the number of tracks in the quarter. Divide the number of tracks by the number of hours calculated above (it's usually around 12-18) and enter that.

That should be it for the webcast and territory report!

#### The Old Boring (And Probably Broken) Way

The data for the territory and hours comes from the icecast logs (stored in ''/var/log/icecast on [](/computing/comp-machines/uryfs1) at the time of writing). These are entered into the database through a script ''processlogs.php. This is quite arcane and fragile, most notably it isn't great at avoiding duplicate entries. So, it looks in a separate folder from where the logs are stored, currently ''/music/streamlogs/database/. Check in the database (''public.strm_log) to see when the last log entry was, and copy all ''access.log- timestamped files after this date (the timestamp is the last day of entry before the log rotated). Run ''php processlogs.php.

Now the logs are in the database, we can get the data we need out of them. Another php script, currently at ''/usr/local/bin/streamstats.php generates this data. It requires ''psql and ''Geo-IP – which should be kept up to date with means of a cron job. Edit ''streamstats.php and set the date ranges in the SQL query to the appropriate quarter. Run the script and copy and paste the bottom of the output (hours and countries) into a spreadsheet, which **should** interpret it as tab separated values.

These values then need transferring to the PPL spreadsheet (ask management to forward it from the quarterly request email). For each country in the PPL sheet, copy the corresponding value from the processed log data. Enter the total hours into the appropriate field. Check that the percentages tally to 100% to ensure that all the listener figures have been accounted for.

Finally, we need 'total tracks per broadcast hour'. First the easy bit – broadcast hour. A broadcast hour is when we are actually broadcasting music, so term time. These **usually** conveniently fall within the respective quarters (Autumn usually straddles Q3 and Q4), but not always, so check the term dates and any 'special events' which were actually broadcast online. So, assuming a single term fits wholly within a quarter, 7 days _ 10 weeks _ 24 hours = 1680 hours.

More tricky – the number of tracks played. If a full tracklist is requested, care must be taken to make sure the same data is used for both, and it is of course good practice to ensure this anyway. Use the MyRadio "full tracklist" option in the [BAPS Statistics](https///ury.org.uk/myradio/Stats/fullTracklist/) page to generate all the tracks played in a quarter (note – the date ranges are a pain in the arse. The best way is to use an unambiguous date range which can't be misinterpreted as either MMDDYY or DDMMYY). Download as a CSV (it should offer this option) and count the number of tracks in the quarter. Divide the number of tracks by the number of hours calculated above (it's usually around 12-18) and enter that.

That should be it for the webcast and territory report!

### 'Full Tracklist' or parts thereof

### Older queries that need sorting

#### Calculating Tracks Played Over Quarter

TODO: Validate this

    :::plsql
    SELECT COUNT(*) FROM baps_audiolog
         INNER JOIN baps_audio USING (audioid)
         WHERE timeplayed >= '2011-01-01' AND timestopped < '2011-04-01'
         AND (timestopped - timeplayed) >= INTERVAL '1 minute'
         AND (filename IS NULL OR
              (filename NOT LIKE '%uryfs1%jingles%'
               AND filename NOT LIKE '%uryfs1%adverts%'
               AND filename NOT LIKE '%uryfs1%beds%'))

#### Calculating Hours Of Show Per Quarter

TODO: Validate this.

    :::plsql
    SELECT SUM(EXTRACT(EPOCH FROM (endtime - starttime)::INTERVAL))/3600
    FROM sis_sign
    INNER JOIN sched_timeslot USING (timeslotid)
    WHERE actiontime >= '20XX-XX-01' AND actiontime < '20XX-XX-01';

#### Calculating Listener-Hours Per Quarter

TODO: Validate this.

    :::plsql
    SELECT SUM(EXTRACT(EPOCH FROM (endtime - starttime)::INTERVAL))/3600
    FROM strm_log
    WHERE starttime >= '20XX-XX-01' AND endtime < '20XX-XX-01'
    AND (endtime - starttime) >= INTERVAL '1 minute'

#### Old stuff that might be better, who knows

Some info needs to be put here

V— This no longer works (strm_log.duration n'exist pas)

You can use this SQL query to get the cumulative listener time for all the streams. Adjust the dates as needed.

    :::plsql
    SELECT SUM(strm_log.duration)
    FROM strm_log INNER JOIN strm_stream USING (streamid)
    WHERE (streamname='/live-high'
    OR streamname='live-low'
    OR streamname = 'broadcast/live.rm'
    OR streamname='/live-high.ogg'
    OR streamname='/live-low.ogg')
    AND duration > INTERVAL '10 seconds'
    AND starttime > '$DATE1'
    AND starttime < '$DATE2'



    :::plsql
    SELECT date_trunc('day', strm_log.starttime), SUM(strm_log.duration)
    FROM strm_log INNER JOIN strm_stream USING (streamid)
    WHERE (streamname='/live-high'
    OR streamname='live-low'
    OR streamname = 'broadcast/live.rm'
    OR streamname='/live-high.ogg'
    OR streamname='/live-low.ogg')
    AND starttime > DATE '2008-01-01'
    AND duration > INTERVAL '30 seconds'
    AND starttime < DATE '2008-06-30'
    AND duration < INTERVAL '24 hours'
    GROUP BY date_trunc
    ORDER BY date_trunc
