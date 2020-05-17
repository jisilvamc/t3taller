<!DOCTYPE html>
<html>
<body>

<h1>Tarea 3 Taller de integración</h1>
<h3>Comentario para los correctores: la interacción es al hacer hover con el mouse en los gráficos, y solo el gráfico de precio vs tiempo es obligatorio en el enunciado :)</h3>
<button onclick="toggle()" id="boton">Detener el websocket (está activado)</button>

<h1>Datos acciones: <h3 id="ntp"></h3></h1>
<script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
<div id="chart0"></div>
<div id="chart1"></div>
<div id="chart2"></div>
<div id="chart3"></div>
<div id="chart4"></div>
<div id="chart5"></div>
<div id="chart6"></div>
<div id="chart7"></div>
<div id="chart8"></div>
<div id="chart9"></div>
<div id="chart10"></div>
<div id="chart11"></div>
<h2>Precio máximo por acción: <h3 id="max"></h3></h2>
<div id="precio_max"></div>
<h2>Precio mínimo por acción: <h3 id="min"></h3></h2>
<div id="precio_min"></div>
<h2>Precio actual por acción: <h3 id="last"></h3></h2>
<div id="precio_actual"></div>
<h2>Porcentaje de cambio por acción: <h3 id="pctg"></h3></h2>
<h2>Volumen de compra por acción: <h3 id="vol_buy"></h3></h2>
<div id="vol_compra"></div>
<h2>Volumen de venta por acción: <h3 id="vol_sell"></h3></h2>
<div id="vol_venta"></div>
<h2>Volumen total por acción: <h3 id="vol"></h3></h2>
<div id="vol_total"></div>

<h1>Datos bolsas: <h3 id="exchange"></h3></h1>
<h2>Volumen de compra por bolsa: <h3 id="vol_buy_ex"></h3></h2>
<div id="vol_compra_bolsa"></div>
<h2>Volumen de venta por bolsa: <h3 id="vol_sell_ex"></h3></h2>
<div id="vol_venta_bolsa"></div>
<h2>Volumen total por bolsa: <h3 id="vol_tot"></h3></h2>
<div id="vol_total_bolsa"></div>
<h2>Participación de mercado por bolsa: <h3 id="part_mer"></h3></h2>
<div id="participacion"></div>



<script src="https://cdn.socket.io/socket.io-1.0.0.js"></script>
<script>


google.charts.load('current', {
  packages: ['corechart', 'line']
});
google.charts.setOnLoadCallback(drawBasic);

function drawBasic(datos, nombre) {
  var data = new google.visualization.DataTable();
  data.addColumn('number', 'X');
  data.addColumn('number', nombre);
  data.addRows(datos);
  var options = {
    hAxis: {
      title: 'Tiempo'
    },
    vAxis: {
      title: 'Precio'
    }
  };
  var chart = new google.visualization.LineChart(document.getElementById('chart'+chart_num[nombre]));
  chart.draw(data, options);
}

const socket = io('wss://le-18262636.bitzonte.com', {
  path: '/stocks',
  transports: ['websocket']
});


var on = 1;
function toggle() {
  if (on == 1) {
    document.getElementById("boton").innerHTML = "Activar el websocket (se mantienen los datos anteriores)";
    on = 0;
    socket.disconnect();
  } else {
    document.getElementById("boton").innerHTML = "Detener el websocket (está activado)";
    on = 1;
    socket.connect();
    socket.emit("STOCKS");
    socket.emit("EXCHANGES");
  }
}


let exchange = new Map(), ntp = new Map();
var nam_tick = {}, hist = {}, chart_cont = 0, chart_num = {}, time_mod = 0;
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

// REVISAR hacer todos los bar charts que faltan; parametrizar ejes y labels, inicializar primera fila especial
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
  if (chart_cont == 0){
    time_mod = data["time"];
  }
  if (!(hist[data["ticker"]])){
    hist[data["ticker"]] = []
    chart_num[data["ticker"]] = chart_cont;
    chart_cont++;
  }
  hist[data["ticker"]].push([(data["time"]%time_mod)/500, data["value"]]);
  drawBasic(hist[data["ticker"]], data["ticker"]);
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