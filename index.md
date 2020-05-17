<!DOCTYPE html>
<html>
<body>

<h1>Tarea 3 Taller de integración</h1>
<h3>Comentario para los correctores: en la pauta y en la rúbrica aparece que el único gráfico obligatorio es el de precio vs tiempo para las acciones :)</h3>
<button onclick="toggle()" id="boton">Detener</button>

<h2>Datos acciones: <h3 id="ntp"></h3></h2>
<h2>Lista bolsas: <h3 id="exchange"></h3></h2>

<h2>Volumen compra por bolsa: <h3 id="vol_buy_ex"></h3></h2>
<h2>Volumen venta por bolsa: <h3 id="vol_sell_ex"></h3></h2>
<h2>Volumen total por bolsa: <h3 id="vol_tot"></h3></h2>
<h2>Participación de mercado por bolsa: <h3 id="part_mer"></h3></h2>

<h2>Precio máximos por acción: <h3 id="max"></h3></h2>
<h2>Precio mínimos por acción: <h3 id="min"></h3></h2>
<h2>Precio actual por acción: <h3 id="last"></h3></h2>
<h2>Porcentaje de cambio por acción: <h3 id="pctg"></h3></h2>
<h2>Volumen compra por acción: <h3 id="vol_buy"></h3></h2>
<h2>Volumen venta por acción: <h3 id="vol_sell"></h3></h2>
<h2>Volumen total por acción: <h3 id="vol"></h3></h2>



<script src="https://cdn.socket.io/socket.io-1.0.0.js"></script>
<script>

    const socket = io('wss://le-18262636.bitzonte.com', {
  path: '/stocks',
  transports: ['websocket']
});

var on = 1;
function toggle() {
  if (on == 1) {
    document.getElementById("boton").innerHTML = "Activar";
    on = 0;
    socket.disconnect();
  } else {
    document.getElementById("boton").innerHTML = "Detener";
    on = 1;
    socket.connect();
    socket.emit("STOCKS");
    socket.emit("EXCHANGES");
    exchange = new Map(), ntp = new Map(), nam_tick = {};
    max = {}, min = {}, vol = {}, vol_buy = {}, vol_sell = {}, last = {}, pctg = {};
    vol_buy_ex = {}, vol_sell_ex = {}, vol_tot = {}, part_mer = {};
    for (var texto of ["max", "min", "last", "pctg", "ntp", "exchange", "vol_buy", "vol_sell", "vol", "vol_buy_ex", "vol_sell_ex", "vol_tot", "part_mer"]){
      document.getElementById(texto).innerHTML = "";
    }
  }
}


let exchange = new Map(), ntp = new Map();
var nam_tick = {};
var max = {}, min = {}, vol = {}, vol_buy = {}, vol_sell = {}, last = {}, pctg = {};
var vol_buy_ex = {}, vol_sell_ex = {}, vol_tot = {}, part_mer = {};

socket.on("STOCKS", (data) => {
  for (var i of data){
    nam_tick[i["company_name"]] = i["ticker"];
    ntp.set(i["company_name"], [i["ticker"], i["country"]])
  }
  var ntp_str = ""
  for (let [key, value] of ntp){
    ntp_str += key + ": " + value + "<br>"
  }
  document.getElementById("ntp").innerHTML = ntp_str;
});

function convert(obj) {
    return Object.keys(obj).map(key => ({
        name: key,
        value: obj[key]
    }));
}

socket.on("EXCHANGES", (data) => {
  var aux = convert(data);
  for (var i of aux){
    var com_tick = [];
    for (j = 0; j < i.value.listed_companies.length; j++) {
      com_tick.push(nam_tick[i.value.listed_companies[j]]);
    }
    exchange.set(i.name, com_tick);
  }
  var exc_str = ""
  for (let [key, value] of exchange){
    exc_str += key + ": " + value + " (" + value.length + " empresas)<br>"
  }
  document.getElementById("exchange").innerHTML = exc_str;
});
socket.emit("STOCKS");
socket.emit("EXCHANGES");

// REVISAR solo se debieran tomar los eventos de las empresas que están en las listas...
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
  document.getElementById("max").innerHTML = JSON.stringify(max);
  document.getElementById("min").innerHTML = JSON.stringify(min);
  document.getElementById("last").innerHTML = JSON.stringify(last);
  document.getElementById("pctg").innerHTML = JSON.stringify(pctg);
});

socket.on("BUY", (data) => {
  if (!(data["ticker"] in vol_buy)) {
    vol_buy[data["ticker"]] = data["volume"];
  } else {
    vol_buy[data["ticker"]] += data["volume"];
  }
  document.getElementById("vol_buy").innerHTML = JSON.stringify(vol_buy);
  if (!(data["ticker"] in vol)) {
    vol[data["ticker"]] = data["volume"];
  } else {
    vol[data["ticker"]] += data["volume"];
  }
  document.getElementById("vol").innerHTML = JSON.stringify(vol);

  // REVISAR agregar part_mer
  for (let [key, value] of exchange){
    if (value.includes(data["ticker"])){
        if (!(key in vol_buy_ex)) {
          vol_buy_ex[key] = data["volume"];
        } else {
          vol_buy_ex[key] += data["volume"];
        }
        document.getElementById("vol_buy_ex").innerHTML = JSON.stringify(vol_buy_ex);
        if (!(key in vol_tot)) {
          vol_tot[key] = data["volume"];
        } else {
          vol_tot[key] += data["volume"];
        }
        document.getElementById("vol_tot").innerHTML = JSON.stringify(vol_tot);
    }
  }

});
socket.on("SELL", (data) => {
  if (!(data["ticker"] in vol_sell)) {
    vol_sell[data["ticker"]] = data["volume"];
  } else {
    vol_sell[data["ticker"]] += data["volume"];
  }
  document.getElementById("vol_sell").innerHTML = JSON.stringify(vol_sell);
  if (!(data["ticker"] in vol)) {
    vol[data["ticker"]] = data["volume"];
  } else {
    vol[data["ticker"]] += data["volume"];
  }
  document.getElementById("vol").innerHTML = JSON.stringify(vol);

  // REVISAR agregar part_mer
    for (let [key, value] of exchange){
      if (value.includes(data["ticker"])){
        if (!(key in vol_sell_ex)) {
          vol_sell_ex[key] = data["volume"];
        } else {
          vol_sell_ex[key] += data["volume"];
        }
        document.getElementById("vol_sell_ex").innerHTML = JSON.stringify(vol_sell_ex);
        if (!(key in vol_tot)) {
          vol_tot[key] = data["volume"];
        } else {
          vol_tot[key] += data["volume"];
        }
        document.getElementById("vol_tot").innerHTML = JSON.stringify(vol_tot);
    }
    for (let [key, value] of exchange){
      if (key == Object.keys(vol_tot)[0]) {
        part_mer[key] = ((vol_tot[key]/(vol_tot[key]+vol_tot[Object.keys(vol_tot)[1]]))*100).toFixed(2)+"%";
      } else {
        part_mer[key] = ((vol_tot[key]/(vol_tot[key]+vol_tot[Object.keys(vol_tot)[0]]))*100).toFixed(2)+"%";
      }
    }
    document.getElementById("part_mer").innerHTML = JSON.stringify(part_mer);
  }
});



</script>

</body>
</html>