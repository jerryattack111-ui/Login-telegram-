import { useEffect, useRef, useState } from "react";

const BS = 28, WW = 200, WH = 64;
const GRAVITY = 0.5, JUMP = -12.5, SPEED = 3.8, MINE_REACH = 6 * BS;
const B = { AIR:0,GRASS:1,DIRT:2,STONE:3,LOG:4,LEAF:5,SAND:6,COAL:7,IRON:8,BEDROCK:9,PLANK:10,GLASS:11 };
const BD = {
  [B.GRASS]:  { c:'#5A9E3A', side:'#7D5A2C', hard:15, name:'Grass' },
  [B.DIRT]:   { c:'#7D5A2C', hard:10, name:'Dirt' },
  [B.STONE]:  { c:'#757575', hard:40, name:'Stone' },
  [B.LOG]:    { c:'#5C3D1E', ring:'#3B2208', hard:20, name:'Log' },
  [B.LEAF]:   { c:'#2A6B1A', hard:5, name:'Leaf' },
  [B.SAND]:   { c:'#D4B44A', hard:8, name:'Sand' },
  [B.COAL]:   { c:'#757575', ore:'#1A1A1A', hard:50, name:'Coal' },
  [B.IRON]:   { c:'#757575', ore:'#C48040', hard:60, name:'Iron Ore' },
  [B.BEDROCK]:{ c:'#111', hard:1e9, name:'Bedrock' },
  [B.PLANK]:  { c:'#C4A055', grain:'#A88040', hard:15, name:'Plank' },
  [B.GLASS]:  { c:'rgba(180,220,255,0.25)', border:'#90C0FF', hard:5, name:'Glass' },
};
const HOTBAR = [B.DIRT,B.STONE,B.SAND,B.LOG,B.PLANK,B.LEAF,B.GLASS];
const HNAMES = ['Dirt','Stone','Sand','Log','Plank','Leaf','Glass'];

function smoothNoise(x,sc,seed){
  const h=n=>{let v=(n^(seed*1234567+891011))*2654435761;v^=v>>16;v*=2246822519;v^=v>>13;return(v&0xffff)/0xffff;};
  const xi=Math.floor(x/sc),xf=x/sc-xi,t=xf*xf*(3-2*xf);
  return h(xi)*(1-t)+h(xi+1)*t;
}
function generateWorld(){
  const data=new Uint8Array(WW*WH), seed=Math.random()*9999|0;
  const get=(x,y)=>(x<0||x>=WW||y<0||y>=WH)?B.BEDROCK:data[y*WW+x];
  const set=(x,y,v)=>{if(x>=0&&x<WW&&y>=0&&y<WH)data[y*WW+x]=v;};
  const heights=Array.from({length:WW},(_,x)=>
    Math.round(WH*0.52-(smoothNoise(x,70,seed)*12+smoothNoise(x,25,seed+1)*5+smoothNoise(x,8,seed+2)*2))
  );
  for(let x=0;x<WW;x++){
    const sy=heights[x];
    for(let y=0;y<WH;y++){
      if(y===WH-1)set(x,y,B.BEDROCK);
      else if(y<sy)set(x,y,B.AIR);
      else if(y===sy)set(x,y,B.GRASS);
      else if(y<sy+4)set(x,y,B.DIRT);
      else set(x,y,B.STONE);
    }
  }
  for(let i=0;i<350;i++){
    const ox=Math.random()*WW|0,oy=(WH*0.6+Math.random()*WH*0.35)|0;
    const tp=Math.random()<0.55?B.COAL:B.IRON;
    for(let j=0;j<(3+Math.random()*4|0);j++){
      const nx=ox+(Math.random()*3|0)-1,ny=oy+(Math.random()*3|0)-1;
      if(get(nx,ny)===B.STONE)set(nx,ny,tp);
    }
  }
  for(let x=3;x<WW-3;x++){
    if(Math.random()<0.07&&get(x,heights[x])===B.GRASS){
      const sy=heights[x],th=4+(Math.random()*3|0);
      for(let j=1;j<=th;j++)set(x,sy-j,B.LOG);
      for(let lx=-2;lx<=2;lx++)for(let ly=-2;ly<=0;ly++){
        if(Math.abs(lx)+Math.abs(ly)<=3&&get(x+lx,sy-th+ly)===B.AIR)set(x+lx,sy-th+ly,B.LEAF);
      }
    }
  }
  return{data,heights,get,set};
}
function isSolid(world,bx,by){const b=world.get(bx,by);return b!==B.AIR&&b!==B.GLASS&&b!==B.LEAF;}
function moveEntity(e,world,dt){
  e.vy=Math.min(e.vy+GRAVITY*dt,20);
  const nx=e.x+e.vx*dt,y1=Math.floor((e.y+4)/BS),y2=Math.floor((e.y+e.h-5)/BS);
  const cx=e.vx>0?Math.floor((nx+e.w)/BS):Math.floor(nx/BS);
  let bX=false;for(let by=y1;by<=y2;by++)if(isSolid(world,cx,by)){bX=true;break;}
  if(!bX)e.x=nx;else{e.vx=0;if(e._bot)e.dir*=-1;}
  const ny=e.y+e.vy*dt,x1=Math.floor((e.x+4)/BS),x2=Math.floor((e.x+e.w-5)/BS);
  const cy=e.vy>0?Math.floor((ny+e.h)/BS):Math.floor(ny/BS);
  let bY=false;for(let bx=x1;bx<=x2;bx++)if(isSolid(world,bx,cy)){bY=true;break;}
  if(!bY){e.y=ny;if(e.vy>0)e.onGround=false;}else{if(e.vy>0)e.onGround=true;e.vy=0;}
  e.x=Math.max(0,Math.min(e.x,(WW-2)*BS));
  e.y=Math.max(-2*BS,Math.min(e.y,(WH-2)*BS));
  if(Math.abs(e.vx)>0.1)e.anim=(e.anim+0.15*dt)%(Math.PI*2);
}
function mkBot(world,name,shirt,pants,ofp){
  const x=Math.floor(WW*(ofp+Math.random()*0.3));
  return{x:x*BS,y:(world.heights[x]-3)*BS,vx:0,vy:0,w:24,h:42,onGround:false,
    name,skin:'#C68642',shirt,pants,dir:1,anim:0,_bot:true,
    botTimer:Math.random()*100|0,health:20,maxHealth:20};
}
function updateBot(bot,world,dt){
  moveEntity(bot,world,dt);bot.botTimer-=dt;
  if(bot.botTimer<=0){
    const r=Math.random();
    if(r<0.35){bot.vx=bot.dir*SPEED;bot.botTimer=50+Math.random()*80;}
    else if(r<0.5){bot.vx=0;bot.botTimer=30+Math.random()*40;}
    else if(r<0.65&&bot.onGround){bot.vy=JUMP;bot.onGround=false;bot.botTimer=20;}
    else{bot.dir=Math.random()<0.5?1:-1;bot.vx=bot.dir*SPEED;bot.botTimer=30+Math.random()*50;}
  }
  if(bot.onGround&&Math.abs(bot.vx)>0.5){
    const fx=Math.floor((bot.x+(bot.dir>0?bot.w+2:-6))/BS),fy=Math.floor((bot.y+bot.h-2)/BS);
    if(isSolid(world,fx,fy)&&!isSolid(world,fx,fy-1)&&!isSolid(world,fx,fy-2)){bot.vy=JUMP;bot.onGround=false;}
  }
}
function drawBlock(ctx,type,px,py,sz,mp,hard){
  const bd=BD[type];if(!bd)return;
  if(type===B.GLASS){
    ctx.fillStyle=bd.c;ctx.fillRect(px,py,sz,sz);
    ctx.strokeStyle=bd.border;ctx.lineWidth=1;ctx.strokeRect(px+0.5,py+0.5,sz-1,sz-1);return;
  }
  ctx.fillStyle=bd.c;ctx.fillRect(px,py,sz,sz);
  if(type===B.GRASS&&bd.side){ctx.fillStyle=bd.side;ctx.fillRect(px,py+sz*0.3,sz,sz*0.7);}
  if(type===B.LOG&&bd.ring){ctx.save();ctx.strokeStyle=bd.ring;ctx.lineWidth=2;ctx.beginPath();ctx.ellipse(px+sz/2,py+sz/2,sz*0.3,sz*0.35,0,0,Math.PI*2);ctx.stroke();ctx.restore();}
  if(type===B.PLANK&&bd.grain){ctx.fillStyle=bd.grain;ctx.fillRect(px,py+sz*0.48,sz,2);}
  if(bd.ore){ctx.fillStyle=bd.ore;[[0.2,0.2],[0.6,0.5],[0.35,0.7],[0.7,0.2]].forEach(([dx,dy])=>ctx.fillRect(px+dx*sz,py+dy*sz,sz*0.15,sz*0.15));}
  ctx.strokeStyle='rgba(0,0,0,0.18)';ctx.lineWidth=0.5;ctx.strokeRect(px+0.5,py+0.5,sz-1,sz-1);
  if(mp>0&&hard>0&&hard<1e9){
    const pct=mp/hard;ctx.save();ctx.beginPath();ctx.rect(px,py,sz,sz);ctx.clip();
    ctx.strokeStyle=`rgba(0,0,0,${pct*0.65})`;ctx.lineWidth=1.5;
    for(let i=0;i<Math.floor(pct*7)+1;i++){
      const cx2=px+sz*0.15+sz*0.7*((i*137.5)%1),cy2=py+sz*0.15+sz*0.7*((i*245.7)%1);
      ctx.beginPath();ctx.moveTo(cx2,cy2);ctx.lineTo(cx2+9-i,cy2+5+i*0.5);ctx.stroke();
    }
    ctx.restore();
  }
}
function drawEntity(ctx,e,cx,cy,isPlayer){
  const x=e.x-cx,y=e.y-cy,w=e.w,h=e.h,sw=Math.sin(e.anim||0)*0.4;
  ctx.save();
  ctx.fillStyle='rgba(0,0,0,0.15)';ctx.beginPath();ctx.ellipse(x+w/2,y+h+2,w*0.45,4,0,0,Math.PI*2);ctx.fill();
  [[0.25,-1],[0.75,1]].forEach(([fx,side])=>{
    ctx.save();ctx.translate(x+w*fx,y+h*0.67);ctx.rotate(side*sw);
    ctx.fillStyle=e.pants||'#1565C0';ctx.fillRect(-w*0.19,0,w*0.38,h*0.33);ctx.restore();
  });
  ctx.fillStyle=e.shirt||'#E53935';ctx.fillRect(x+w*0.1,y+h*0.3,w*0.8,h*0.38);
  [[0.04,-1],[0.96,1]].forEach(([fx,side])=>{
    ctx.save();ctx.translate(x+w*fx,y+h*0.3);ctx.rotate(side*sw);
    ctx.fillStyle=e.shirt||'#E53935';ctx.fillRect(-w*0.11,0,w*0.22,h*0.35);ctx.restore();
  });
  const hw=w*0.76,hh=h*0.28,hx=x+w/2-hw/2,hy=y;
  ctx.fillStyle=e.skin||'#C68642';ctx.fillRect(hx,hy,hw,hh);
  if(isPlayer){ctx.fillStyle='#8B0000';ctx.fillRect(hx-2,hy-4,hw+4,hh*0.4);}
  const eo=e.dir>0?0.55:0.25;
  ctx.fillStyle='#FFF';ctx.fillRect(hx+hw*eo,hy+hh*0.28,hw*0.18,hh*0.26);
  ctx.fillStyle='#111';ctx.fillRect(hx+hw*(eo+(e.dir>0?0.08:-0.02)),hy+hh*0.34,hw*0.08,hh*0.16);
  ctx.font='bold 10px monospace';const tw=ctx.measureText(e.name).width;
  ctx.fillStyle='rgba(0,0,0,0.55)';ctx.fillRect(x+w/2-tw/2-5,y-21,tw+10,16);
  ctx.fillStyle='#FFF';ctx.textAlign='center';ctx.fillText(e.name,x+w/2,y-8);
  ctx.restore();
}

// ── Touch joystick state (module-level mutable refs) ──
const touch = {
  joystick: { active:false, startX:0, startY:0, dx:0, dy:0, id:-1 },
  mineActive: false,
  placeActive: false,
};

export default function MinecraftGame(){
  const canvasRef=useRef(null);
  const overlayRef=useRef(null);
  const [mode,setMode]=useState('mine'); // 'mine' | 'place'
  const [slot,setSlot]=useState(0);
  const modeRef=useRef('mine');
  const slotRef=useRef(0);

  useEffect(()=>{modeRef.current=mode;},[mode]);
  useEffect(()=>{slotRef.current=slot;},[slot]);

  useEffect(()=>{
    const canvas=canvasRef.current;
    let cw=canvas.width=window.innerWidth, ch=canvas.height=window.innerHeight;
    const ctx=canvas.getContext('2d');
    const world=generateWorld();
    const cx2=WW/2|0;
    const player={
      x:cx2*BS,y:(world.heights[cx2]-3)*BS,vx:0,vy:0,w:24,h:42,
      onGround:false,name:'You',skin:'#C68642',shirt:'#E53935',pants:'#1565C0',
      dir:1,selectedSlot:0,mineX:-1,mineY:-1,mineP:0,health:20,maxHealth:20,anim:0,
    };
    const bots=[
      mkBot(world,'Steve','#3F51B5','#795548',0.15),
      mkBot(world,'Alex','#4CAF50','#6D4C41',0.4),
      mkBot(world,'Notch','#9C27B0','#212121',0.65),
    ];
    bots[1].skin='#B5A48B';
    const state={
      world,player,bots,camera:{x:0,y:0},
      keys:{},chat:[
        {text:'Welcome to Minecraft 2D!',time:Date.now()},
        {text:'Steve joined the game',time:Date.now()+300},
        {text:'Alex joined the game',time:Date.now()+600},
        {text:'Notch joined the game',time:Date.now()+900},
        {text:'You joined the game',time:Date.now()+1200},
      ],
      dayTime:0.5,tick:0,
      mineTarget:{x:-1,y:-1},mineHold:false,
    };

    // ── Keyboard ──
    const onKD=e=>{
      state.keys[e.code]=true;
      if((e.code==='Space'||e.code==='ArrowUp'||e.code==='KeyW')&&player.onGround){player.vy=JUMP;player.onGround=false;}
      if(e.code>='Digit1'&&e.code<='Digit7'){slotRef.current=+e.code[5]-1;setSlot(slotRef.current);}
    };
    const onKU=e=>state.keys[e.code]=false;

    // ── Mouse (desktop) ──
    const mouseState={x:cw/2,y:ch/2,left:false};
    const onMM=e=>{const r=canvas.getBoundingClientRect();mouseState.x=e.clientX-r.left;mouseState.y=e.clientY-r.top;};
    const onMD=e=>{
      if(e.button===0)mouseState.left=true;
      if(e.button===2){
        const wx=Math.floor((mouseState.x+state.camera.x)/BS);
        const wy=Math.floor((mouseState.y+state.camera.y)/BS);
        const dist=Math.hypot((wx+0.5)*BS-player.x-player.w/2,(wy+0.5)*BS-player.y-player.h/2);
        if(dist<MINE_REACH&&world.get(wx,wy)===B.AIR){
          world.set(wx,wy,HOTBAR[slotRef.current]);
          state.chat.push({text:`You placed ${HNAMES[slotRef.current]}`,time:Date.now()});
          while(state.chat.length>10)state.chat.shift();
        }
        e.preventDefault();
      }
    };
    const onMU=e=>{if(e.button===0){mouseState.left=false;player.mineX=-1;player.mineY=-1;player.mineP=0;}};
    const onWhl=e=>{slotRef.current=(slotRef.current+(e.deltaY>0?1:-1)+HOTBAR.length)%HOTBAR.length;setSlot(slotRef.current);e.preventDefault();};

    // ── Touch ──
    // Joystick zone: left 40% of screen
    // Jump button: right side top
    // Mine/Place toggle: handled by React buttons in overlay
    // Canvas tap: mine or place depending on mode

    const JOY_R=60; // joystick radius
    const JOY_CX_RATIO=0.12, JOY_CY_RATIO=0.78; // base position ratios

    const getTouchTarget=(tx,ty)=>{
      // Is it in joystick zone?
      const jcx=cw*JOY_CX_RATIO, jcy=ch*JOY_CY_RATIO;
      if(tx<cw*0.35&&ty>ch*0.5) return 'joystick';
      return 'world';
    };

    const onTS=e=>{
      e.preventDefault();
      for(const t of e.changedTouches){
        const tx=t.clientX,ty=t.clientY;
        const target=getTouchTarget(tx,ty);
        if(target==='joystick'&&!touch.joystick.active){
          touch.joystick.active=true;touch.joystick.id=t.identifier;
          touch.joystick.startX=tx;touch.joystick.startY=ty;
          touch.joystick.dx=0;touch.joystick.dy=0;
        } else if(target==='world'){
          // Tap on world = mine or place
          state.mineTarget={x:tx,y:ty};
          state.mineHold=true;
          state.mineMode=modeRef.current;
        }
      }
    };
    const onTM=e=>{
      e.preventDefault();
      for(const t of e.changedTouches){
        if(t.identifier===touch.joystick.id){
          const dx=t.clientX-touch.joystick.startX, dy=t.clientY-touch.joystick.startY;
          const len=Math.hypot(dx,dy);
          const clamped=Math.min(len,JOY_R);
          touch.joystick.dx=len>0?(dx/len)*clamped:0;
          touch.joystick.dy=len>0?(dy/len)*clamped:0;
        } else {
          state.mineTarget={x:t.clientX,y:t.clientY};
        }
      }
    };
    const onTE=e=>{
      e.preventDefault();
      for(const t of e.changedTouches){
        if(t.identifier===touch.joystick.id){
          touch.joystick.active=false;touch.joystick.dx=0;touch.joystick.dy=0;touch.joystick.id=-1;
        } else {
          // If it was a short tap on world and in place mode, place block
          if(modeRef.current==='place'){
            const wx=Math.floor((t.clientX+state.camera.x)/BS);
            const wy=Math.floor((t.clientY+state.camera.y)/BS);
            const dist=Math.hypot((wx+0.5)*BS-player.x-player.w/2,(wy+0.5)*BS-player.y-player.h/2);
            if(dist<MINE_REACH&&world.get(wx,wy)===B.AIR){
              const px3=Math.floor(player.x/BS),py3=Math.floor(player.y/BS);
              if(!(wx>=px3-1&&wx<=px3+1&&wy>=py3-1&&wy<=py3+2)){
                world.set(wx,wy,HOTBAR[slotRef.current]);
                state.chat.push({text:`You placed ${HNAMES[slotRef.current]}`,time:Date.now()});
                while(state.chat.length>10)state.chat.shift();
              }
            }
          }
          state.mineHold=false;player.mineX=-1;player.mineY=-1;player.mineP=0;
        }
      }
    };

    canvas.addEventListener('touchstart',onTS,{passive:false});
    canvas.addEventListener('touchmove',onTM,{passive:false});
    canvas.addEventListener('touchend',onTE,{passive:false});
    canvas.addEventListener('touchcancel',onTE,{passive:false});
    canvas.addEventListener('mousemove',onMM);
    canvas.addEventListener('mousedown',onMD);
    canvas.addEventListener('mouseup',onMU);
    canvas.addEventListener('wheel',onWhl,{passive:false});
    canvas.addEventListener('contextmenu',e=>e.preventDefault());
    window.addEventListener('keydown',onKD);
    window.addEventListener('keyup',onKU);
    const onRsz=()=>{cw=canvas.width=window.innerWidth;ch=canvas.height=window.innerHeight;};
    window.addEventListener('resize',onRsz);

    // Jump button ref handler (exposed via window)
    window._mcJump=()=>{if(player.onGround){player.vy=JUMP;player.onGround=false;}};

    let raf,lastT=0;
    function loop(t){
      const dt=Math.min((t-lastT)/16.67,3);lastT=t;
      state.tick+=dt;state.dayTime=(state.dayTime+dt*0.0005)%1;
      player.selectedSlot=slotRef.current;

      // Keyboard movement
      const k=state.keys;
      let moved=false;
      if(k['ArrowLeft']||k['KeyA']){player.vx=-SPEED;player.dir=-1;moved=true;}
      else if(k['ArrowRight']||k['KeyD']){player.vx=SPEED;player.dir=1;moved=true;}
      // Joystick movement
      if(touch.joystick.active){
        const jdx=touch.joystick.dx, jdy=touch.joystick.dy;
        const deadzone=10;
        if(Math.abs(jdx)>deadzone){
          player.vx=(jdx/60)*SPEED*2.2;
          player.dir=jdx>0?1:-1;
          moved=true;
        } else if(!moved) player.vx*=0.7;
        if(jdy<-40&&player.onGround){player.vy=JUMP;player.onGround=false;}
      } else if(!moved) player.vx=0;

      if((k['Space']||k['ArrowUp']||k['KeyW'])&&player.onGround){player.vy=JUMP;player.onGround=false;}
      moveEntity(player,world,dt);

      // Mining (mouse)
      if(mouseState.left){
        const wx=Math.floor((mouseState.x+state.camera.x)/BS);
        const wy=Math.floor((mouseState.y+state.camera.y)/BS);
        const dist=Math.hypot((wx+0.5)*BS-player.x-player.w/2,(wy+0.5)*BS-player.y-player.h/2);
        if(dist<MINE_REACH){
          const bt=world.get(wx,wy);
          if(bt!==B.AIR&&BD[bt]){
            if(player.mineX!==wx||player.mineY!==wy){player.mineX=wx;player.mineY=wy;player.mineP=0;}
            player.mineP+=dt;
            if(player.mineP>=BD[bt].hard&&BD[bt].hard<1e9){
              world.set(wx,wy,B.AIR);
              state.chat.push({text:`You mined ${BD[bt].name}`,time:Date.now()});
              while(state.chat.length>10)state.chat.shift();
              player.mineX=-1;player.mineY=-1;player.mineP=0;
            }
          }
        }
      }
      // Mining (touch)
      if(state.mineHold&&state.mineMode==='mine'){
        const tx=state.mineTarget.x, ty=state.mineTarget.y;
        const wx=Math.floor((tx+state.camera.x)/BS);
        const wy=Math.floor((ty+state.camera.y)/BS);
        const dist=Math.hypot((wx+0.5)*BS-player.x-player.w/2,(wy+0.5)*BS-player.y-player.h/2);
        if(dist<MINE_REACH){
          const bt=world.get(wx,wy);
          if(bt!==B.AIR&&BD[bt]){
            if(player.mineX!==wx||player.mineY!==wy){player.mineX=wx;player.mineY=wy;player.mineP=0;}
            player.mineP+=dt;
            if(player.mineP>=BD[bt].hard&&BD[bt].hard<1e9){
              world.set(wx,wy,B.AIR);
              state.chat.push({text:`You mined ${BD[bt].name}`,time:Date.now()});
              while(state.chat.length>10)state.chat.shift();
              player.mineX=-1;player.mineY=-1;player.mineP=0;
            }
          }
        }
      }

      bots.forEach(b=>updateBot(b,world,dt));
      if(Math.random()<0.0008*dt){
        const msgs=['Found diamonds!','Building my house!','Watch out!','Hello world!','Mining away~'];
        const b=bots[Math.floor(Math.random()*bots.length)];
        state.chat.push({text:`${b.name}: ${msgs[Math.floor(Math.random()*msgs.length)]}`,time:Date.now()});
        while(state.chat.length>10)state.chat.shift();
      }

      state.camera.x=Math.max(0,Math.min(player.x-cw/2+player.w/2,WW*BS-cw));
      state.camera.y=Math.max(0,Math.min(player.y-ch/2+player.h/2,WH*BS-ch));

      // ── Render ──
      const cam=state.camera;
      const dayA=Math.sin(state.dayTime*Math.PI*2)*0.5+0.5;
      const r2=Math.round(15+dayA*110),g2=Math.round(25+dayA*150),b2=Math.round(45+dayA*170);
      ctx.fillStyle=`rgb(${r2},${g2},${b2})`;ctx.fillRect(0,0,cw,ch);
      if(dayA<0.4){
        ctx.fillStyle=`rgba(255,255,255,${(0.4-dayA)*2.5})`;
        for(let i=0;i<70;i++){const sx=((i*137.5*cw/70+cam.x*0.01)%cw+cw)%cw,sy=(i*73.1*ch/70)%ch*0.55;ctx.fillRect(sx,sy,1.5,1.5);}
      }
      const sA=state.dayTime*Math.PI*2;
      const sunX=cw/2+Math.cos(sA-Math.PI/2)*cw*0.42,sunY=ch*0.5+Math.sin(sA-Math.PI/2)*ch*0.42;
      if(dayA>0.1){ctx.fillStyle='#FFD54F';ctx.beginPath();ctx.arc(sunX,sunY,28,0,Math.PI*2);ctx.fill();}
      const mnX=cw/2-Math.cos(sA-Math.PI/2)*cw*0.42,mnY=ch*0.5-Math.sin(sA-Math.PI/2)*ch*0.42;
      if(dayA<0.9){ctx.fillStyle='#E0E0E0';ctx.beginPath();ctx.arc(mnX,mnY,20,0,Math.PI*2);ctx.fill();}
      if(dayA>0.15){
        ctx.fillStyle=`rgba(255,255,255,${(dayA-0.15)*0.6})`;
        [0.08,0.38,0.63,0.82].forEach((cf,i)=>{
          const clx=((cf*cw*1.5-cam.x*0.06)%(cw*1.5)+cw*1.5)%(cw*1.5)-cw*0.25,cly=40+i*20;
          ctx.beginPath();ctx.ellipse(clx,cly,60+i*8,14,0,0,Math.PI*2);ctx.fill();
          ctx.beginPath();ctx.ellipse(clx+40,cly-5,38,11,0,0,Math.PI*2);ctx.fill();
        });
      }
      const sxB=Math.max(0,Math.floor(cam.x/BS)-1),exB=Math.min(WW,sxB+Math.ceil(cw/BS)+3);
      const syB=Math.max(0,Math.floor(cam.y/BS)-1),eyB=Math.min(WH,syB+Math.ceil(ch/BS)+3);
      for(let bx=sxB;bx<exB;bx++)for(let by=syB;by<eyB;by++){
        const tp=world.get(bx,by);if(tp===B.AIR)continue;
        const mp=(player.mineX===bx&&player.mineY===by)?player.mineP:0;
        drawBlock(ctx,tp,bx*BS-cam.x,by*BS-cam.y,BS,mp,BD[tp]?.hard||0);
      }
      bots.forEach(b=>drawEntity(ctx,b,cam.x,cam.y,false));
      drawEntity(ctx,player,cam.x,cam.y,true);

      // Cursor highlight (desktop)
      const wx2=Math.floor((mouseState.x+cam.x)/BS),wy2=Math.floor((mouseState.y+cam.y)/BS);
      const cd=Math.hypot((wx2+0.5)*BS-player.x-player.w/2,(wy2+0.5)*BS-player.y-player.h/2);
      if(cd<MINE_REACH){
        if(world.get(wx2,wy2)!==B.AIR){ctx.strokeStyle='rgba(255,255,255,0.7)';ctx.lineWidth=2;ctx.strokeRect(wx2*BS-cam.x,wy2*BS-cam.y,BS,BS);}
        else{ctx.strokeStyle='rgba(255,255,255,0.3)';ctx.lineWidth=1;ctx.strokeRect(wx2*BS-cam.x+0.5,wy2*BS-cam.y+0.5,BS-1,BS-1);}
      }
      // Touch mine target highlight
      if(state.mineHold){
        const tx=state.mineTarget.x,ty=state.mineTarget.y;
        const twx=Math.floor((tx+cam.x)/BS),twy=Math.floor((ty+cam.y)/BS);
        ctx.strokeStyle='rgba(255,80,80,0.8)';ctx.lineWidth=2;ctx.strokeRect(twx*BS-cam.x,twy*BS-cam.y,BS,BS);
      }

      // Draw joystick
      if(touch.joystick.active){
        const jcx=touch.joystick.startX,jcy=touch.joystick.startY;
        ctx.save();
        ctx.strokeStyle='rgba(255,255,255,0.3)';ctx.lineWidth=2;
        ctx.beginPath();ctx.arc(jcx,jcy,60,0,Math.PI*2);ctx.stroke();
        ctx.fillStyle='rgba(255,255,255,0.18)';ctx.beginPath();ctx.arc(jcx,jcy,60,0,Math.PI*2);ctx.fill();
        const knobX=jcx+touch.joystick.dx,knobY=jcy+touch.joystick.dy;
        ctx.fillStyle='rgba(255,255,255,0.5)';ctx.beginPath();ctx.arc(knobX,knobY,28,0,Math.PI*2);ctx.fill();
        ctx.strokeStyle='rgba(255,255,255,0.6)';ctx.lineWidth=2;ctx.beginPath();ctx.arc(knobX,knobY,28,0,Math.PI*2);ctx.stroke();
        ctx.restore();
      } else {
        // Static joystick hint
        const jcx=cw*JOY_CX_RATIO,jcy=ch*JOY_CY_RATIO;
        ctx.save();
        ctx.strokeStyle='rgba(255,255,255,0.12)';ctx.lineWidth=1.5;
        ctx.beginPath();ctx.arc(jcx,jcy,55,0,Math.PI*2);ctx.stroke();
        ctx.fillStyle='rgba(255,255,255,0.07)';ctx.beginPath();ctx.arc(jcx,jcy,55,0,Math.PI*2);ctx.fill();
        ctx.fillStyle='rgba(255,255,255,0.1)';ctx.beginPath();ctx.arc(jcx,jcy,24,0,Math.PI*2);ctx.fill();
        ctx.restore();
      }

      // Chat
      const now=Date.now();const vis=state.chat.filter(m=>now-m.time<6000);
      ctx.font='11px monospace';ctx.textAlign='left';
      vis.forEach((msg,i)=>{
        const al=Math.max(0,1-(now-msg.time)/6000),idx=vis.length-1-i;
        const tw2=ctx.measureText(msg.text).width;
        ctx.fillStyle=`rgba(0,0,0,${al*0.5})`;ctx.fillRect(8,ch-130-idx*17,tw2+10,14);
        ctx.fillStyle=`rgba(255,255,255,${al})`;ctx.fillText(msg.text,13,ch-130-idx*17+10);
      });

      // Coords
      const hr=Math.floor(state.dayTime*24),mn2=Math.floor((state.dayTime*24-hr)*60);
      ctx.fillStyle='rgba(0,0,0,0.5)';ctx.fillRect(cw-148,8,140,48);
      ctx.fillStyle='rgba(255,255,255,0.9)';ctx.font='10px monospace';ctx.textAlign='left';
      ctx.fillText(`X:${Math.floor(player.x/BS)} Y:${Math.floor(player.y/BS)}`,cw-142,23);
      ctx.fillText(`🕐 ${String(hr).padStart(2,'0')}:${String(mn2).padStart(2,'0')}`,cw-142,40);

      raf=requestAnimationFrame(loop);
    }
    raf=requestAnimationFrame(loop);
    return()=>{
      cancelAnimationFrame(raf);
      canvas.removeEventListener('touchstart',onTS);
      canvas.removeEventListener('touchmove',onTM);
      canvas.removeEventListener('touchend',onTE);
      canvas.removeEventListener('touchcancel',onTE);
      canvas.removeEventListener('mousemove',onMM);
      canvas.removeEventListener('mousedown',onMD);
      canvas.removeEventListener('mouseup',onMU);
      canvas.removeEventListener('wheel',onWhl);
      window.removeEventListener('keydown',onKD);
      window.removeEventListener('keyup',onKU);
      window.removeEventListener('resize',onRsz);
    };
  },[]);

  return (
    <div style={{position:'relative',width:'100vw',height:'100vh',overflow:'hidden',background:'#87CEEB',userSelect:'none',WebkitUserSelect:'none'}}>
      <canvas ref={canvasRef} style={{display:'block',touchAction:'none',cursor:'crosshair'}} />

      {/* ── Mobile UI Overlay ── */}
      <div ref={overlayRef} style={{position:'absolute',inset:0,pointerEvents:'none'}}>

        {/* Jump button */}
        <button
          onPointerDown={e=>{e.stopPropagation();window._mcJump&&window._mcJump();}}
          style={{
            position:'absolute',right:22,bottom:200,
            width:70,height:70,borderRadius:'50%',
            background:'rgba(0,0,0,0.45)',border:'2px solid rgba(255,255,255,0.35)',
            color:'#FFF',fontSize:26,display:'flex',alignItems:'center',justifyContent:'center',
            pointerEvents:'auto',cursor:'pointer',touchAction:'manipulation',
            boxShadow:'0 2px 12px rgba(0,0,0,0.3)',
          }}
        >⬆</button>

        {/* Mine / Place toggle */}
        <div style={{position:'absolute',right:22,bottom:100,display:'flex',flexDirection:'column',gap:10,pointerEvents:'auto'}}>
          <button
            onPointerDown={e=>{e.stopPropagation();setMode('mine');}}
            style={{
              width:64,height:64,borderRadius:14,
              background:mode==='mine'?'rgba(229,57,53,0.85)':'rgba(0,0,0,0.45)',
              border:`2px solid ${mode==='mine'?'#FF8A80':'rgba(255,255,255,0.3)'}`,
              color:'#FFF',fontSize:22,cursor:'pointer',touchAction:'manipulation',
              boxShadow:mode==='mine'?'0 0 14px rgba(229,57,53,0.5)':'none',
            }}
          >⛏</button>
          <button
            onPointerDown={e=>{e.stopPropagation();setMode('place');}}
            style={{
              width:64,height:64,borderRadius:14,
              background:mode==='place'?'rgba(67,160,71,0.85)':'rgba(0,0,0,0.45)',
              border:`2px solid ${mode==='place'?'#69F0AE':'rgba(255,255,255,0.3)'}`,
              color:'#FFF',fontSize:22,cursor:'pointer',touchAction:'manipulation',
              boxShadow:mode==='place'?'0 0 14px rgba(67,160,71,0.5)':'none',
            }}
          >🧱</button>
        </div>

        {/* Mode label */}
        <div style={{
          position:'absolute',right:100,bottom:mode==='mine'?134:70,
          background:'rgba(0,0,0,0.6)',color:'#FFF',padding:'3px 10px',
          borderRadius:20,fontSize:11,fontFamily:'monospace',pointerEvents:'none',
          border:`1px solid ${mode==='mine'?'rgba(229,57,53,0.5)':'rgba(67,160,71,0.5)'}`,
          whiteSpace:'nowrap',
        }}>
          {mode==='mine'?'⛏ MINE (hold)':'🧱 PLACE (tap)'}
        </div>

        {/* Hotbar */}
        <div style={{
          position:'absolute',bottom:8,left:'50%',transform:'translateX(-50%)',
          display:'flex',gap:5,padding:'5px 8px',
          background:'rgba(0,0,0,0.55)',borderRadius:12,
          border:'1px solid rgba(255,255,255,0.2)',pointerEvents:'auto',
        }}>
          {HOTBAR.map((bt,i)=>(
            <div key={i} onPointerDown={e=>{e.stopPropagation();setSlot(i);slotRef.current=i;}}
              style={{
                width:46,height:46,borderRadius:8,position:'relative',cursor:'pointer',
                background:slot===i?'rgba(255,255,255,0.22)':'rgba(60,60,60,0.4)',
                border:`2px solid ${slot===i?'#FFF':'rgba(255,255,255,0.15)'}`,
                display:'flex',flexDirection:'column',alignItems:'center',justifyContent:'center',
                boxShadow:slot===i?'0 0 10px rgba(255,255,255,0.4)':'none',
                transition:'all 0.12s',
              }}>
              <canvas width={30} height={30} ref={el=>{
                if(!el)return;
                const c=el.getContext('2d');c.clearRect(0,0,30,30);
                drawBlock(c,bt,1,1,28,0,0);
              }} style={{imageRendering:'pixelated'}} />
              <span style={{position:'absolute',bottom:1,left:2,fontSize:8,color:'rgba(255,255,255,0.6)',fontFamily:'monospace',lineHeight:1}}>
                {HNAMES[i].slice(0,4)}
              </span>
            </div>
          ))}
        </div>

        {/* Health hearts */}
        <div style={{
          position:'absolute',top:8,left:'50%',transform:'translateX(-50%)',
          display:'flex',gap:2,pointerEvents:'none',
        }}>
          {Array.from({length:10},(_,i)=>(
            <span key={i} style={{fontSize:14,filter:i<10?'none':'grayscale(1)'}}>❤️</span>
          ))}
        </div>

        {/* Touch hint (first few seconds) */}
        <div style={{
          position:'absolute',top:'50%',left:'50%',transform:'translate(-50%,-50%)',
          background:'rgba(0,0,0,0.7)',borderRadius:16,padding:'16px 24px',
          border:'1px solid rgba(255,255,255,0.2)',pointerEvents:'none',
          animation:'fadeout 0.6s 3.5s forwards',
          textAlign:'center',color:'#FFF',fontFamily:'monospace',
        }}>
          <div style={{fontSize:20,marginBottom:8}}>⛏ Minecraft 2D</div>
          <div style={{fontSize:12,lineHeight:1.8,opacity:0.9}}>
            🕹 Left side — Joystick<br/>
            ⬆ Right — Jump<br/>
            ⛏ Hold to mine &nbsp;|&nbsp; 🧱 Tap to place<br/>
            Steve, Alex & Notch are with you!
          </div>
        </div>
      </div>

      <style>{`
        @keyframes fadeout{from{opacity:1}to{opacity:0;pointer-events:none}}
        button:active{transform:scale(0.92);}
      `}</style>
    </div>
  );
}
