let CONFIG = {
  accuWeatherAPIKEY: "YOUR-ACCUWEATHER-API-KEY", 
  weatherCurrentEndpoint:
    "http://dataservice.accuweather.com/currentconditions/v1/",
  locations: {
    Sesto_San_Giovanni: 213640,
    Milano: 2541071
  },
  shutters: [
   "YOUR-SHELLY-2.5-IP"
  ],
  closeShutterAction : "roller/0?go=to_pos&roller_pos=100",
  //check every 60 seconds * 30 => 30 min
  checkInterval: 60 * 30 * 1000,
};

function getWeatherURLForLocation(location) {
  let api=
    CONFIG.weatherCurrentEndpoint +
    JSON.stringify(CONFIG.locations[location]) +
    "?apikey=" +
    CONFIG.accuWeatherAPIKEY +
    "&details=false" +
    "language=it-it";
  print("making the request... ");
  print(api);
  return api;
}

function WeatherControlLocation(location) {
   print("check if raining at ", location);
  Shelly.call(
    "http.get",
    { url: getWeatherURLForLocation(location) },
    function (response, error_code, error_message) {
      let data= JSON.parse(response.body);
      print("The response is ",JSON.stringify(data));
      if (data.HasPrecipitation === true) {
          closeShutters(true);
        }
      else {
          print("currently it's not raining'");
          }
      }
  );
}

function closeShutters(){
   for(let i =0; i< CONFIG.rollers.shutters; i++){
     Shelly.call(
    "http.get",
    { url: CONFIG.shutters[i] + CONFIG.closeShutterAction },
    function (response, error_code, error_message, location) {
      let data= JSON.parse(response.body);
          print(location, "I am closing shutters");
      }
     );
   }
}

//WeatherControlLocation("Sesto_San_Giovanni");
Timer.set(CONFIG.checkInterval, true, function () {
  console.log("checking weather");
  WeatherControlLocation("Sesto_San_Giovanni");
});

