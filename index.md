<!DOCTYPE html>
<html>
<body>

<h1>Tarea 3 Taller de integración</h1>

<script src="https://cdn.socket.io/socket.io-1.0.0.js"></script>
<script>

  const socket = io('wss://le-18262636.bitzonte.com', {
  path: '/stocks',
  transports: ['websocket']
});

var max = {}, min = {}, vol_buy = {}, vol_sell = {}, last = {}, pctg = {};  // calcular vol total al mostrar
var vol_buy_ex = {}, vol_sell_ex = {}, vol_tot = {}, num_acc = {}, part_mer = {};  // num_acc es número de empresas

// REVISAR
socket.on("EXCHANGES", (data) => {
  console.log("EXCHANGES");
  console.log(data);
});
socket.on("STOCKS", (data) => {
  console.log("STOCKS");
  console.log(data);
});
var tiempo = 1000;
socket.emit("STOCKS");
socket.emit("EXCHANGES");

// REVISAR solo se toman los eventos de las empresas que están en las listas...
socket.on("UPDATE", (data) => {
  if (data["ticker"] in max) {
  	if (max[data["ticker"]] < data["value"]) {
  		max[data["ticker"]] = data["value"];
  		}
  	if (min[data["ticker"]] > data["value"]) {
  		min[data["ticker"]] = data["value"];
  		}
  } else {
  	max[data["ticker"]] = data["value"];
  	min[data["ticker"]] = data["value"];
  }
  if (!(data["ticker"] in last)) {
  	last[data["ticker"]] = data["value"];
  }
  pctg[data["ticker"]] = ((([data["value"]]-last[data["ticker"]])/last[data["ticker"]])*100).toFixed(2)+"%";
  last[data["ticker"]] = data["value"];
});
socket.on("BUY", (data) => {
	if (!(data["ticker"] in vol_buy)) {
		vol_buy[data["ticker"]] = data["volume"];
	} else {
		vol_buy[data["ticker"]] += data["volume"];
	}
});
socket.on("SELL", (data) => {
	if (! (data["ticker"] in vol_sell)) {
		vol_sell[data["ticker"]] = data["volume"];
	} else {
		vol_sell[data["ticker"]] += data["volume"];
	}
});

</script>

</body>
</html>