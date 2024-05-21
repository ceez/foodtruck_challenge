# foodtruck_challenge
Utilize data from Opendata site data.sfgov.org.

This project will consist of proof of concept demo, that it production ready.
For ease of setup the API will be built in a Docker container.

## Assumptions, Constraints and Observations

- This project must use perl. 
- The best choices for this type of project seem to be dancer2 and
  mojolicius.  These are the most similar to Flask and FastAPI from
  Python, which I have been using lately.
- The project is free form, but must utilize data located on the data.sfgov.org, and specifically this particular
  dataset, https://data.sfgov.org/Economy-and-Community/Mobile-Food-Facility-Permit/rqzj-sfat/data

### Backgroup about the data source

- The data consists of public, open records. It is Read Only for this project
- At the time of the start of this project, there are a total of 481
  records.  Attributes or columns may be prefixed with a ':'.  This
  indicates that it is metadata added by Socrata system, and includes
  a unique row identifies, along with a creation and change time (akin
  to file mtime).
- The data is hosted by Tyler Technologies (formerly Socrata), and is
  accessible via the SODA Consumer API.  It documented here:
  https://dev.socrata.com/consumers/getting-started.html, , and it
  provides for two types of queries:
  + Simple filters
  + Socrata Query Language (SoQL) - and SQL-like domain specific language

#### curl example for a Socrata Query

The command cherry picking the first row with jq.
``` bash
$ curl -Gs  --data-urlencode '$select=:*, *'  "https://data.sfgov.org/resource/rqzj-sfat.json" | jq '.[0]'
```

The result
``` json 
{
  ":id": "row-iuec-rtpx-37j2",
  ":created_at": "2021-10-14T16:49:18.877Z",
  ":updated_at": "2021-10-14T16:49:22.883Z",
  ":version": "rv-pkwp~rtjn_rtx5",
  ":@computed_region_yftq_j783": "4",
  ":@computed_region_p5aj_wyqh": "1",
  ":@computed_region_rxqg_mtj9": "10",
  ":@computed_region_bh8s_q3mv": "28855",
  ":@computed_region_fyvs_ahh9": "6",
  "objectid": "735318",
  "applicant": "Ziaurehman Amini",
  "facilitytype": "Push Cart",
  "cnn": "30727000",
  "locationdescription": "MARKET ST: DRUMM ST intersection",
  "address": "5 THE EMBARCADERO",
  "blocklot": "0234017",
  "block": "0234",
  "lot": "017",
  "permit": "15MFF-0159",
  "status": "REQUESTED",
  "x": "6013916.72",
  "y": "2117244.027",
  "latitude": "37.794331003246846",
  "longitude": "-122.39581105302317",
  "schedule": "http://bsm.sfdpw.org/PermitsTracker/reports/report.aspx?title=schedule&report=rptSchedule&params=permit=15MFF-0159&ExportPDF=1&Filename=15MFF-0159_schedule.pdf",
  "received": "20151231",
  "priorpermit": "0",
  "expirationdate": "2016-03-15T00:00:00.000",
  "location": {
    "latitude": "37.794331003246846",
    "longitude": "-122.39581105302317",
    "human_address": "{\"address\": \"\", \"city\": \"\", \"state\": \"\", \"zip\": \"\"}"
  }
}
```

#### Joined data
- Examination of the exported data shows some additional data is "joined" to other sources.

``` text
Fire Prevention Districts
:@computed_region_yftq_j783
Police Districts
:@computed_region_p5aj_wyqh
Supervisor Districts
:@computed_region_rxqg_mtj9
Zip Codes
:@computed_region_bh8s_q3mv
Neighborhoods (old)
:@computed_region_fyvs
```

### API Constraints
- The documentation mentions that results from the API are typically
  paged, and will return a maximum of 50K records per page.
- The API is throttled to 10000 requests per rolling hour period. 

# Security

- 



  
  
