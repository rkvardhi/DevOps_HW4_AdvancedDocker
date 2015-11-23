var express = require('express')
var fs      = require('fs')
var app = express()

var args = process.argv.slice(2);
var PORT = args[0];

app.get('/', function(req, res) 
{
	fs.appendFile('message.txt', 'Sample message\n', function (err) {
  		if (err) throw err;
  		console.log('Sample message was appended to file!');
	});
	
	res.send("Hello");
});

var server = app.listen(PORT, function () {

	var host = server.address().address
	var port = server.address().port
	fs.writeFile('message.txt', 'Some text in file\n', function (err) {
  		if (err) throw err;
  		console.log('File was created and some text is written to it');
	});
	console.log('Example app listening at http://%s:%s', host, port)
});
