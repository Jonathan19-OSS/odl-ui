<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>OpenDaylight</title>
<style>
  :root{--odl-orange:#f5a623;--bg:#2e2e2e;--sidebar:#4a4a4a;--panel:#3a3a3a}
  body{margin:0;font:13px 'Segoe UI',Arial;background:var(--bg);color:#ddd;overflow:hidden}
  header{height:48px;background:#1c1c1c;border-bottom:3px solid #d32f2f;display:flex;align-items:center;padding:0 16px}
  header img{height:28px;margin-right:8px}
  header .title{color:var(--odl-orange);font-size:16px;font-weight:300}
  header .right{margin-left:auto;color:#ccc;font-size:12px;cursor:pointer}
  
  .login-wrap{display:grid;place-items:center;height:calc(100vh - 48px);background:#eee}
  .login{background:#fff;border:1px solid #ccc;border-radius:4px;width:360px;box-shadow:0 2px 8px rgba(0,0,0,.2)}
  .login h3{background:#f5f5f5;margin:0;padding:10px;border-bottom:1px solid #ddd;font-weight:400;color:#333}
  .login-body{padding:16px}
  .login input{width:100%;padding:8px;margin:8px 0;border:1px solid #ccc;border-radius:3px;box-sizing:border-box}
  .login button{width:100%;padding:10px;border:0;border-radius:3px;background:var(--odl-orange);color:#000;font-weight:700;cursor:pointer;margin-top:8px}
  .login label{font-size:12px;color:#333}
  
  .app{display:none;height:calc(100vh - 48px)}
  .row{display:flex;height:100%}
  .sidebar{width:200px;background:var(--sidebar);padding-top:8px}
  .sidebar a{display:block;padding:10px 16px;color:#ccc;text-decoration:none;cursor:pointer;font-size:13px}
  .sidebar a.active{background:#666;color:#fff}
  .sidebar a:hover{background:#555}
  .main{flex:1;background:var(--panel);padding:16px;overflow:auto}
  .card{background:#fff;color:#333;border-radius:3px;padding:12px}
  h4{color:#fff;margin:0 0 10px 0}
  table{width:100%;border-collapse:collapse;font-size:12px}
  th{background:var(--odl-orange);color:#000;padding:8px;text-align:left;font-weight:600}
  td{padding:8px;border-bottom:1px solid #ddd;background:#fff;color:#333}
  td a{color:#007bff;text-decoration:none}
  .search{padding:6px;width:200px;border:1px solid #555;background:#555;color:#fff;margin-bottom:10px}
  #topo{width:100%;height:500px;background:#fff;border:1px solid #ccc}
</style>
<script src="https://unpkg.com/vis-network/standalone/umd/vis-network.min.js"></script>
</head>
<body>

<header>
  <img src="https://www.opendaylight.org/wp-content/uploads/2017/01/odl-logo.png">
  <div class="title">OPEN DAYLIGHT</div>
  <div class="right" id="logout" style="display:none">☰ Logout (admin)</div>
</header>

<!-- LOGIN -->
<div class="login-wrap" id="login">
  <div class="login">
    <h3>Please Sign In</h3>
    <div class="login-body">
      <input id="u" value="admin">
      <input id="p" type="password" value="admin">
      <label><input type="checkbox"> Remember Me</label>
      <button onclick="auth()">Login</button>
    </div>
  </div>
</div>

<!-- APP -->
<div class="app" id="app">
  <div class="row">
    <aside class="sidebar">
      <a class="active" onclick="showTab('topo',this)">Topology</a>
      <a onclick="showTab('nodes',this)">Nodes</a>
      <a onclick="showTab('flows',this)">Flows</a>
    </aside>
    <section class="main">
      <!-- TOPOLOGY -->
      <div id="topo-tab">
        <button style="padding:6px 12px;background:#666;color:#fff;border:0;margin-bottom:8px;cursor:pointer">Reload</button>
        <div id="topo" class="card"></div>
      </div>
      <!-- NODES -->
      <div id="nodes-tab" style="display:none">
        <input class="search" placeholder="Search Nodes">
        <table>
          <tr><th>Node Id</th><th>Node Name</th><th>Node Connectors</th><th>Statistics</th></tr>
          <tr><td>openflow:1</td><td>s1</td><td>3</td><td><a href="#" onclick="showStats()">Flows</a> | <a href="#" onclick="showStats()">Node Connectors</a></td></tr>
          <tr><td>openflow:2</td><td>s2</td><td>3</td><td><a href="#">Flows</a> | <a href="#">Node Connectors</a></td></tr>
          <tr><td>openflow:3</td><td>s3</td><td>3</td><td><a href="#">Flows</a> | <a href="#">Node Connectors</a></td></tr>
          <tr><td>openflow:4</td><td>s4</td><td>3</td><td><a href="#">Flows</a> | <a href="#">Node Connectors</a></td></tr>
        </table>
      </div>
      <!-- STATS -->
      <div id="stats-tab" style="display:none">
        <h4>Node Connector Statistics for Node Id - openflow:1</h4>
        <table>
          <tr><th>Node Connector Id</th><th>Rx Pkts</th><th>Tx Pkts</th><th>Rx Bytes</th><th>Tx Bytes</th><th>Rx Drops</th><th>Tx Drops</th><th>Rx Errs</th><th>Tx Errs</th><th>Rx Frame Err</th><th>Rx Overrun Err</th><th>Rx CRC Err</th><th>Collisions</th></tr>
          <tr><td>openflow:1:1</td><td>17</td><td>185</td><td>1218</td><td>15196</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
          <tr><td>openflow:1:2</td><td>185</td><td>167</td><td>15396</td><td>13968</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
          <tr><td>openflow:1:LOCAL</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
        </table>
        <button onclick="showTab('nodes',document.querySelector('.sidebar a:nth-child(2)'))" style="margin-top:10px;padding:6px 12px">Back</button>
      </div>
      <!-- FLOWS -->
      <div id="flows-tab" style="display:none">
        <h4>Flows for Node: openflow:1</h4>
        <table>
          <tr><th>Flow Id</th><th>Table</th><th>Priority</th><th>Match</th><th>Instructions</th><th>Packets</th><th>Bytes</th></tr>
          <tr><td>1</td><td>0</td><td>100</td><td>eth_type=0x0800,ip_dst=10.0.0.2</td><td>output:2</td><td>185</td><td>15396</td></tr>
          <tr><td>2</td><td>0</td><td>100</td><td>eth_type=0x0800,ip_dst=10.0.0.3</td><td>output:3</td><td>167</td><td>13968</td></tr>
          <tr><td>3</td><td>0</td><td>0</td><td></td><td>drop</td><td>17</td><td>1218</td></tr>
        </table>
      </div>
    </section>
  </div>
</div>

<script>
function auth(){
  if(document.getElementById('u').value==='admin' && document.getElementById('p').value==='admin'){
    document.getElementById('login').style.display='none';
    document.getElementById('app').style.display='block';
    document.getElementById('logout').style.display='block';
  }else{alert('Mauvais login. Use: admin / admin')}
}
function showTab(id, el){
  ['topo','nodes','stats','flows'].forEach(t=>{
    document.getElementById(t+'-tab').style.display = t===id ? 'block' : 'none';
  });
  if(el) document.querySelectorAll('.sidebar a').forEach(a=>a.classList.remove('active')), el.classList.add('active');
}
function showStats(){showTab('stats',null)}

// Topo Base Station comme ta photo 2
const baseIcon = 'https://i.imgur.com/8Qf5Q3b.png'; // Icône antenne 3D
const nodes = new vis.DataSet([
  {id:1,label:'BASE STATION\nLUBUMBASHI',image:baseIcon,shape:'image',x:-200,y:-100},
  {id:2,label:'BASE STATION\nKAMALONDO',image:baseIcon,shape:'image',x:0,y:-150},
  {id:3,label:'BASE STATION\nNYOTA',image:baseIcon,shape:'image',x:200,y:-100},
  {id:4,label:'BASE STATION\nKAMISEGA',image:baseIcon,shape:'image',x:-250,y:50},
  {id:5,label:'BASE STATION\nKASAPA',image:baseIcon,shape:'image',x:-100,y:100},
  {id:6,label:'BASE STATION\nKENYA',image:baseIcon,shape:'image',x:50,y:50},
]);
const edges = new vis.DataSet([
  {from:1,to:2,label:'Point to point',color:{color:'#f5a623'},arrows:'to',font:{color:'red',align:'top'}},
  {from:2,to:3,label:'Point to point',color:{color:'#f5a623'},arrows:'to',font:{color:'red',align:'top'}},
  {from:1,to:4,label:'Point to point',color:{color:'#2ecc71'},arrows:'to',font:{color:'red',align:'left'}},
  {from:2,to:5,label:'Station procure',color:{color:'#f5a623'},arrows:'to',font:{color:'red',align:'top'}},
  {from:4,to:5,label:'Point to point',color:{color:'#f5a623'},arrows:'to',font:{color:'red',align:'bottom'}},
  {from:5,to:6,label:'Point to point',color:{color:'#f5a623'},arrows:'to',font:{color:'red',align:'bottom'}},
  {from:3,to:6,label:'Point to point',color:{color:'#f5a623'},arrows:'to',font:{color:'red',align:'right'}},
]);
new vis.Network(document.getElementById('topo'), {nodes,edges}, {
  physics:false, 
  edges:{font:{size:12}},
  interaction:{dragNodes:true, zoomView:true}
});
</script>
</body>
</html>
