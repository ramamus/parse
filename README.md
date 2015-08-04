# parse
Parse.Cloud.afterSave("Prescription", function(request) {  
  var rxNumber = request.object.get('RxNumber');
  var rxStatus = request.object.get('Status');
  var rxChannel = request.object.get('channel');
 
  var pushQuery = new Parse.Query(Parse.Installation);
  pushQuery.equalTo('channels', rxChannel);
     
  Parse.Push.send({
    where: pushQuery, // Set our Installation query
    data: {
      alert:  "Your Prescription " + rxNumber + " is " + rxStatus,
    badge: "Increment",
    sound: "cheering.caf",
    title: "RxCyborg"
    }
  }, {
    success: function() {
      // Push was successful
    },
    error: function(error) {
      throw "Got an error " + error.code + " : " + error.message;
    }
  });
});
 
Parse.Cloud.define("hello", function(request, response) {
  response.success("Hello world!");
});
