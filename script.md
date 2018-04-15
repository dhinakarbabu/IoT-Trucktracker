# IoT-Trucktracker
JavaScript to get values from the cloud server and depicting them in the web application
<script src="http://maps.googleapis.com/maps/api/js?key=AIzaSyAva2ZWRsDqRSTGbtIULovadpgXkHngPLc&sensor=false"></script>
<script type="text/javascript">
        var myCenter;
        var map;
        var mapProp;
        var index = 0;
        var x;
        var y;
        var z;
        var lat = [];
        var lng = [];
        function initialize()
        {
            const url = 'https://api.thingspeak.com/channels/436615/feeds.json?api_key="Key"&results=2';
            fetch(url)
           .then((resp) => resp.json())
           .then(function(data) {
                      x= data.feeds[1].field1;
                      y= data.feeds[1].field2;
                      z= data.feeds[1].field3;
                      lat[index] = data.feeds[1].field4;
        	      lng[index] = data.feeds[1].field5;
                      index++;
              myCenter =new google.maps.LatLng(lat[index-1],lng[index-1]);
              mapProp = {
              center:myCenter,
              zoom:15,
              mapTypeId:google.maps.MapTypeId.ROADMAP
              };
            setmap();
                     dir_update();
                     veh_mov();
             mark();
                 })
           setInterval('fetch_data()',25000);
          }
        function setmap(){
                            map=new google.maps.Map(document.getElementById("googleMap"),mapProp);
                                     }
        function mark()
        {
                          var  marker=new google.maps.Marker({
                             position:new google.maps.LatLng(lat[index-1],lng[index-1] ),
                             map: map
                              });
          }
     function fetch_data()
       {
           const url = 'https://api.thingspeak.com/channels/436615/feeds.json?api_key=LK91YI755KBPMXY2&results=2';
           fetch(url)
           .then((resp) => resp.json())
           .then(function(data) {
                      x= data.feeds[1].field1;
                      y= data.feeds[1].field2;
                      z= data.feeds[1].field3;
                       lat[index] = data.feeds[1].field4;
        	      lng[index] = data.feeds[1].field5;   
                      index++;
                     dir_update();
                     veh_mov();
                     mark();
             })
        } 
          function  dir_update()
          { 
                   if(parseFloat(x) >300.000 || parseFloat(y) > 300.000 || parseFloat(z)>300.000){
                      document.getElementById("direction").innerHTML = "<span style='color: red;'>Wrong Direction</span>"; 
                      }
                     else{
                       document.getElementById("direction").innerHTML ="<span style='color: green;'> Normal</span>";  
                                }
                 }
     function  veh_mov(){
            if(index<=1){
               document.getElementById("status").innerHTML = "<span style='color: red;'>Not moving</span>";
               }
            else{
               if(parseFloat(lat[index]) == parseFloat(lat[index-1]) &&  parseFloat(lng[index]) == parseFloat(lng[index-1])){
                    document.getElementById("status").innerHTML = "<span style='color: red;'>   Haulted</span>";
                    }
                 else{
                         document.getElementById("status").innerHTML = "<span style='color: green;'></span>";
                        } 
                   }
          }             
google.maps.event.addDomListener(window, 'load', initialize);
    </script>
