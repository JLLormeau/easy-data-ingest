# Data Ingest

In this lab you will manually ingest data directly with Dynatrace Saas as entry point, or with active gate as entry point and finally directly from the OneAgent without Token.                  

## Entry point = Dynatrace Saas
- Export the variables

      export MyTenant=<MyTenant>
      export MyToken=<MyToken>
      export URL_Saas=https://$MyTenant/api/v2/metrics/ingest
      export Header="Content-Type: text/plain; charset=utf-8"
      export Metric="truck.fuel.total,trucknr=99,model=mac-granite 10234"

- Verify the variables 

      echo "MyTenant=https://"$MyTenant;echo "MyToken="$MyToken;echo "URL_Saas="$URL_Saas;echo "Header="$Header;echo "Metric="$Metric 

- Run the data ingest

      curl -H "Authorization: Api-Token "$MyToken"" -X POST -H "$Header" --data-ascii "$Metric" "$URL_Saas"
 
## Entry point = ActiveGate 
- Export the variables

      export MyToken=<MyToken>
      export Host_AG=<Host_AG>
      export URL_AG=https://$Host_AG:9999/e/1234/api/v2/metrics/ingest
      export Header="Content-Type: text/plain; charset=utf-8"
      export Metric="truck.fuel.total,trucknr=99,model=mac-granite 10234"

- Verify the variables 

      echo "Host_AG=https://"$Host_AG;echo "MyToken="$MyToken;echo "URL_AG="$URL_AG;echo "Header="$Header;echo "Metric="$Metric 


- Run the data ingest

      curl -H "Authorization: Api-Token "$MyToken"" -X POST -H "$Header" --data-ascii "$Metric" "$URL_AG" --insecure

## Entry point = OneAgent
- Prerequisite
  Go to `Settings > Monitoring > Monitored technologies` => In the list of supported technologies, search for the Dynatrace OneAgent StatsD, Pipe, HTTP Metric API entry.  
  The metric is automatically linked to this host 

- Export the variables

      export URL_OA=http://localhost:14499/metrics/ingest
      export Header="Content-Type: text/plain; charset=utf-8"
      export Metric="truck.fuel.total,trucknr=99,model=mac-granite 10234"

- Run the data ingest

      curl -X POST  -H "$Header" --data-ascii "$Metric" "$URL_OA"
 

## Attached to an entityid

- **host**
            
      export Metric="truck.fuel.total,dt.entity.host=HOST-XXXXXXX,trucknr=99,model=mac-granite 10234"

to obtain the value of the HOST-XXXX, click on the Host on Dynatarce and find the HOST-XXXXX directly in the URL parameters:  
![image](https://user-images.githubusercontent.com/40337213/120121394-7ca5c200-c1a3-11eb-80c2-e081ae6cbde5.png)
Run the data ingest for a host : 

      curl -H "Authorization: Api-Token "$Api-Token"" -X POST -H "$Header" --data-ascii "$Metric" "$URL_Saas"

- **custom devive**
   Run the dataingest for a custom device : 

      export Metric="truck.fuel.total,dt.entity.custom_device=CUSTOM_DEVICE-XXXXXXXX,trucknr=99,model=mac-granite 10234"
  to create a custom devive, open 3technologie" and clic on "Custom Device" + [...] + New Custom Device
![image](https://user-images.githubusercontent.com/40337213/120234328-06af6280-c258-11eb-9b8e-cb21c0e6bcea.png)

      curl -H "Authorization: Api-Token "$Api-Token"" -X POST -H "$Header" --data-ascii "$Metric" "$URL_Saas"

- **entityid list** 

      dt.entity.host 
      dt.entity.process_group_instance
      dt.entity.process_group
      dt.entity.service
      dt.entity.application
      dt.entity.custom_device
      dt.entity.custom_device_group
      dt.entity.aws_load_balancer

# Run the script to generate continue data ingest

    cd ~
    git clone https://github.com/JLLormeau/easy-data-ingest
    cd easy-data-ingest/data-ingest
    chmod +x data-ingest-easy-shipping-ltd.sh
    ./data-ingest-easy-shipping-ltd.sh &
      
      
# Next Step
- [Topology Model](/topology-model) : organise the metrics