import { useState, useEffect, useRef } from "react"

const GENS = [
  { id:0,  nome:"Semente",          emoji:"🌱", pps:1,          base:50           },
  { id:1,  nome:"Canteiro",         emoji:"🌿", pps:5,          base:250          },
  { id:2,  nome:"Jardim",           emoji:"🍃", pps:15,         base:1500         },
  { id:3,  nome:"Fazenda",          emoji:"🚜", pps:50,         base:8000         },
  { id:4,  nome:"Estufa",           emoji:"🏡", pps:160,        base:45000        },
  { id:5,  nome:"Fábrica",          emoji:"🏭", pps:500,        base:200000       },
  { id:6,  nome:"Lab Genético",     emoji:"🔬", pps:2000,       base:1500000      },
  { id:7,  nome:"Clonagem",         emoji:"⚗️", pps:7500,       base:10000000     },
  { id:8,  nome:"Portal Quântico",  emoji:"🌀", pps:30000,      base:75000000     },
  { id:9,  nome:"Gerador Quântico", emoji:"⚡", pps:150000,     base:500000000    },
  { id:10, nome:"Usina Cósmica",    emoji:"🌌", pps:750000,     base:4000000000   },
  { id:11, nome:"Dimensão Fruta",   emoji:"🪐", pps:4000000,    base:30000000000  },
  { id:12, nome:"Omniverso Verde",  emoji:"♾️", pps:25000000,   base:250000000000 },
]

const UPS = [
  { id:0,  nome:"Mãos Fortes",           emoji:"🤝", desc:"Endurece suas mãos para colheitas eficientes.",           bonus:1,          custo:100            },
  { id:1,  nome:"Luvas Profissionais",   emoji:"🧤", desc:"Maximiza a força e precisão de cada colheita.",            bonus:4,          custo:600            },
  { id:2,  nome:"Faca Especial",         emoji:"🔪", desc:"Faca afiada própria para colher melancias.",              bonus:10,         custo:3000           },
  { id:3,  nome:"Ferramenta Dourada",    emoji:"🛠️", desc:"Ferramenta dourada que dobra sua eficiência.",            bonus:25,         custo:15000          },
  { id:4,  nome:"Instinto Natural",      emoji:"🌾", desc:"Anos de prática tornam você expert em melancias.",        bonus:65,         custo:80000          },
  { id:5,  nome:"Técnica Avançada",      emoji:"📐", desc:"Técnicas modernas de agricultura de precisão.",           bonus:175,        custo:700000         },
  { id:6,  nome:"Sabedoria Ancestral",   emoji:"📜", desc:"Segredos milenares de cultivo de melancias.",             bonus:500,        custo:5000000        },
  { id:7,  nome:"Colheita Lunar",        emoji:"🌙", desc:"Usa ciclos lunares para potencializar a colheita.",       bonus:1500,       custo:40000000       },
  { id:8,  nome:"Energia Cósmica",       emoji:"✨", desc:"Canaliza energia do cosmos em cada clique.",              bonus:4500,       custo:350000000      },
  { id:9,  nome:"Toque Divino",          emoji:"🙏", desc:"Seu toque materializa melancias do próprio éter.",        bonus:14000,      custo:3000000000     },
  { id:10, nome:"Dedos de Aço",          emoji:"🦾", desc:"Seus dedos ficaram duros como metal puro.",               bonus:45000,      custo:25000000000    },
  { id:11, nome:"Velocidade Sônica",     emoji:"⚡", desc:"Você clica mais rápido do que o som.",                    bonus:150000,     custo:200000000000   },
  { id:12, nome:"Fúria da Melancia",     emoji:"💢", desc:"A energia das melancias flui pelo seu corpo inteiro.",    bonus:500000,     custo:1500000000000  },
  { id:13, nome:"Singularidade Fruta",   emoji:"🌀", desc:"Cada clique dobra toda a realidade frutífera.",           bonus:2000000,    custo:12000000000000 },
  { id:14, nome:"Benção da Semente",     emoji:"🌟", desc:"A primeira semente do universo abençoa seus cliques.",    bonus:8000000,    custo:1e14           },
  { id:15, nome:"Pulsão Dimensional",    emoji:"🌌", desc:"Cada clique ressoa em todas as dimensões ao mesmo tempo.",bonus:35000000,   custo:9e14           },
  { id:16, nome:"Mão do Criador",        emoji:"🖐️", desc:"Você se torna o criador de melancias de todo o cosmos.", bonus:150000000,  custo:8e15           },
  { id:17, nome:"Eco do Infinito",       emoji:"♾️", desc:"Cada toque ecoa infinitamente pelo tecido do universo.", bonus:700000000,  custo:7e16           },
  { id:18, nome:"Toque da Eternidade",   emoji:"⏳", desc:"Sua existência inteira vira energia de melancia pura.",  bonus:3500000000, custo:6e17           },
  { id:19, nome:"O Clique Absoluto",     emoji:"💥", desc:"O golpe que transcende o tempo, espaço e a razão.",      bonus:20000000000,custo:5e18           },
]

const CONQS = [
  { id:0,  emoji:"🌱", nome:"Primeira Melancia",     desc:"Colha sua primeira melancia",              req:1,           tipo:"total",    premio:5        },
  { id:1,  emoji:"🍉", nome:"Entusiasta",             desc:"Colha 100 melancias",                      req:100,         tipo:"total",    premio:20       },
  { id:2,  emoji:"🍉", nome:"Colecionador",           desc:"Colha 1.000 melancias",                    req:1e3,         tipo:"total",    premio:75       },
  { id:3,  emoji:"🍉", nome:"Acumulador",             desc:"Colha 10.000 melancias",                   req:1e4,         tipo:"total",    premio:250      },
  { id:4,  emoji:"🍉", nome:"Fanático",               desc:"Colha 100.000 melancias",                  req:1e5,         tipo:"total",    premio:700      },
  { id:5,  emoji:"🍉", nome:"Milionário",             desc:"Colha 1 milhão de melancias",              req:1e6,         tipo:"total",    premio:3000     },
  { id:6,  emoji:"🍉", nome:"Bilionário",             desc:"Colha 1 bilhão de melancias",              req:1e9,         tipo:"total",    premio:60000    },
  { id:7,  emoji:"🍉", nome:"Trilionário",            desc:"Colha 1 trilhão de melancias",             req:1e12,        tipo:"total",    premio:500000   },
  { id:8,  emoji:"👑", nome:"Deus das Melancias",     desc:"Colha 1 quatrilhão de melancias",          req:1e15,        tipo:"total",    premio:5000000  },
  { id:9,  emoji:"👆", nome:"Clicador Iniciante",     desc:"Clique 50 vezes",                          req:50,          tipo:"cliques",  premio:30       },
  { id:10, emoji:"👆", nome:"Clicador Dedicado",      desc:"Clique 500 vezes",                         req:500,         tipo:"cliques",  premio:150      },
  { id:11, emoji:"👆", nome:"Clicador Máster",        desc:"Clique 5.000 vezes",                       req:5000,        tipo:"cliques",  premio:800      },
  { id:12, emoji:"👆", nome:"Lenda do Clique",        desc:"Clique 50.000 vezes",                      req:50000,       tipo:"cliques",  premio:5000     },
  { id:13, emoji:"👆", nome:"Clicador Olímpico",      desc:"Clique 200.000 vezes",                     req:200000,      tipo:"cliques",  premio:30000    },
  { id:14, emoji:"🏅", nome:"Um Milhão de Cliques",   desc:"Clique 1.000.000 vezes",                   req:1e6,         tipo:"cliques",  premio:200000   },
  { id:15, emoji:"💀", nome:"Dedo Imortal",           desc:"Clique 10.000.000 vezes",                  req:1e7,         tipo:"cliques",  premio:2000000  },
  { id:16, emoji:"⚙️", nome:"Agricultor",             desc:"Compre seu primeiro gerador",              req:1,           tipo:"geradores",premio:50       },
  { id:17, emoji:"⚙️", nome:"Grande Fazendeiro",      desc:"Tenha 10 geradores no total",              req:10,          tipo:"geradores",premio:300      },
  { id:18, emoji:"⚙️", nome:"Magnata Rural",          desc:"Tenha 50 geradores no total",              req:50,          tipo:"geradores",premio:2000     },
  { id:19, emoji:"⚙️", nome:"Imperador das Frutas",   desc:"Tenha 200 geradores no total",             req:200,         tipo:"geradores",premio:20000    },
  { id:20, emoji:"🌎", nome:"Deus do Cultivo",        desc:"Tenha 500 geradores no total",             req:500,         tipo:"geradores",premio:150000   },
  { id:21, emoji:"♾️", nome:"Omnifazendeiro",         desc:"Tenha 1.000 geradores no total",           req:1000,        tipo:"geradores",premio:1000000  },
  { id:22, emoji:"⬆️", nome:"Melhorias Iniciais",     desc:"Compre 3 upgrades",                        req:3,           tipo:"upgrades", premio:200      },
  { id:23, emoji:"⬆️", nome:"Técnico Dedicado",       desc:"Compre 8 upgrades",                        req:8,           tipo:"upgrades", premio:5000     },
  { id:24, emoji:"⬆️", nome:"Especialista",           desc:"Compre 14 upgrades",                       req:14,          tipo:"upgrades", premio:50000    },
  { id:25, emoji:"🌟", nome:"Colecionador de Poder",  desc:"Compre todos os 20 upgrades",              req:20,          tipo:"upgrades", premio:1000000  },
  { id:26, emoji:"📈", nome:"Produção Básica",        desc:"Atinja 100/s de produção",                 req:100,         tipo:"pps",      premio:1000     },
  { id:27, emoji:"📈", nome:"Fábrica Pequena",        desc:"Atinja 10.000/s",                          req:10000,       tipo:"pps",      premio:20000    },
  { id:28, emoji:"📈", nome:"Megafazenda",            desc:"Atinja 1.000.000/s",                       req:1e6,         tipo:"pps",      premio:500000   },
  { id:29, emoji:"🚀", nome:"Força Cósmica",          desc:"Atinja 1 bilhão/s",                        req:1e9,         tipo:"pps",      premio:50000000 },
  { id:30, emoji:"🔄", nome:"Primeira Renascença",    desc:"Faça seu primeiro Rebirth",                req:1,           tipo:"rebirth",  premio:0        },
  { id:31, emoji:"🔄", nome:"Fênix da Melancia",      desc:"Faça 5 Rebirths",                          req:5,           tipo:"rebirth",  premio:0        },
  { id:32, emoji:"🔄", nome:"Ciclo Eterno",           desc:"Faça 10 Rebirths",                         req:10,          tipo:"rebirth",  premio:0        },
  { id:33, emoji:"🔄", nome:"Transcendência Total",   desc:"Faça 25 Rebirths",                         req:25,          tipo:"rebirth",  premio:0        },
  { id:34, emoji:"🍀", nome:"Sorte de Iniciante",     desc:"Colha 500 melancias de uma vez",           req:500,         tipo:"saldo",    premio:100      },
  { id:35, emoji:"💰", nome:"Rico",                   desc:"Tenha 100.000 melancias ao mesmo tempo",   req:100000,      tipo:"saldo",    premio:5000     },
  { id:36, emoji:"🏦", nome:"Bilhardário",            desc:"Tenha 1 bilhão ao mesmo tempo",            req:1e9,         tipo:"saldo",    premio:100000   },
]

const REBIRTH_UPS = [
  { id:0, nome:"Impulso da Fênix",       desc:"Produção base +25% permanente",            emoji:"🔥", bonus:0.25, tipo:"prod",    custo:1  },
  { id:1, nome:"Raiz Profunda",          desc:"Multiplicador de clique +50%",              emoji:"🌳", bonus:0.50, tipo:"click",   custo:2  },
  { id:2, nome:"Fênix Dupla",            desc:"Produção base +50% permanente",             emoji:"🐦", bonus:0.50, tipo:"prod",    custo:3  },
  { id:3, nome:"Solo Fértil",            desc:"Todos geradores custam 10% menos",          emoji:"🌍", bonus:0.10, tipo:"desconto",custo:4  },
  { id:4, nome:"Explosão Celeste",       desc:"Multiplicador de clique +100%",             emoji:"💫", bonus:1.00, tipo:"click",   custo:5  },
  { id:5, nome:"Semente Estelar",        desc:"Produção base +100% permanente",            emoji:"⭐", bonus:1.00, tipo:"prod",    custo:6  },
  { id:6, nome:"Eco da Eternidade",      desc:"Ganho de sementes +50% por rebirth",        emoji:"♾️", bonus:0.50, tipo:"seeds",   custo:8  },
  { id:7, nome:"Toque do Cosmos",        desc:"Multiplicador de clique +200%",             emoji:"🌌", bonus:2.00, tipo:"click",   custo:10 },
  { id:8, nome:"Desconto Profundo",      desc:"Geradores custam 20% menos",                emoji:"💸", bonus:0.20, tipo:"desconto",custo:12 },
  { id:9, nome:"Renascimento Absoluto",  desc:"Prod +200% e Clique +200% (TOTAL)",         emoji:"💥", bonus:2.00, tipo:"all",     custo:20 },
]

const fmt = (n) => {
  n = Math.floor(n)
  if (n >= 1e18) return (n/1e18).toFixed(2)+"Qi"
  if (n >= 1e15) return (n/1e15).toFixed(2)+"Qa"
  if (n >= 1e12) return (n/1e12).toFixed(2)+"T"
  if (n >= 1e9)  return (n/1e9).toFixed(2)+"B"
  if (n >= 1e6)  return (n/1e6).toFixed(2)+"M"
  if (n >= 1e3)  return (n/1e3).toFixed(1)+"K"
  return n.toString()
}

const SR = ({ l, v, sp }) => (
  <div className="flex justify-between items-center py-1 border-b border-white/5 text-xs">
    <span style={{color:"#7a9e7a"}}>{l}</span>
    <span style={{color:sp?"#f0a020":"#e8e8e8",fontWeight:600}}>{v}</span>
  </div>
)
const SS = ({ title, children }) => (
  <div className="mb-3">
    <div style={{color:"#4a7a4a",fontSize:"10px",fontWeight:800,letterSpacing:"0.15em",textTransform:"uppercase",marginBottom:3}}>{title}</div>
    {children}
  </div>
)

export default function App() {
  const [mel, setMel]           = useState(0)
  const [total, setTotal]       = useState(0)
  const [clicks, setClicks]     = useState(0)
  const [cash, setCash]         = useState(0)
  const [tab, setTab]           = useState("casa")
  const [gens, setGens]         = useState(GENS.map(g=>({...g,qtd:0,custo:g.base})))
  const [ups, setUps]           = useState(UPS.map(u=>({...u,ok:false})))
  const [conqs, setConqs]       = useState(CONQS.map(c=>({...c,ok:false})))
  const [rups, setRups]         = useState(REBIRTH_UPS.map(r=>({...r,ok:false})))
  const [rebirths, setRebirths] = useState(0)
  const [seeds, setSeeds]       = useState(0)
  const [totalSeeds, setTotalSeeds] = useState(0)
  const [floats, setFloats]     = useState([])
  const [toasts, setToasts]     = useState([])
  const [wiggle, setWiggle]     = useState(false)
  const [confirm, setConfirm]   = useState(false)
  const [anim, setAnim]         = useState(false)
  const ppsRef = useRef(0), pcRef = useRef(1), fid = useRef(0), tid = useRef(0)

  const prodMult  = 1 + rups.filter(r=>r.ok&&(r.tipo==="prod"||r.tipo==="all")).reduce((s,r)=>s+r.bonus,0)
  const clickMult = 1 + rups.filter(r=>r.ok&&(r.tipo==="click"||r.tipo==="all")).reduce((s,r)=>s+r.bonus,0)
  const discount  = rups.filter(r=>r.ok&&r.tipo==="desconto").reduce((s,r)=>s+r.bonus,0)
  const seedMult  = 1 + rups.filter(r=>r.ok&&r.tipo==="seeds").reduce((s,r)=>s+r.bonus,0)
  const pc = Math.floor(ups.filter(u=>u.ok).reduce((s,u)=>s+u.bonus,1)*clickMult)
  const pps = gens.reduce((s,g)=>s+g.pps*g.qtd,0)*prodMult
  const tGens = gens.reduce((s,g)=>s+g.qtd,0)
  const tUps = ups.filter(u=>u.ok).length
  const seedPrev = Math.max(0, Math.floor(Math.sqrt(total/1e6)*seedMult))

  ppsRef.current = pps; pcRef.current = pc

  useEffect(()=>{
    const iv = setInterval(()=>{
      const g = ppsRef.current/20
      if (g<=0) return
      setMel(m=>m+g); setTotal(t=>t+g)
    },50)
    return ()=>clearInterval(iv)
  },[])

  useEffect(()=>{
    setConqs(cs=>{
      const award=[]
      const next=cs.map(c=>{
        if(c.ok) return c
        const met=(c.tipo==="total"&&total>=c.req)||(c.tipo==="cliques"&&clicks>=c.req)||
          (c.tipo==="geradores"&&tGens>=c.req)||(c.tipo==="upgrades"&&tUps>=c.req)||
          (c.tipo==="rebirth"&&rebirths>=c.req)||(c.tipo==="pps"&&pps>=c.req)||
          (c.tipo==="saldo"&&mel>=c.req)
        if(!met) return c
        award.push(c); return {...c,ok:true}
      })
      if(award.length>0){
        const bon=award.reduce((s,c)=>s+c.premio,0)
        if(bon>0){setMel(m=>m+bon);setTotal(t=>t+bon)}
        award.forEach(c=>toast(`🏆 ${c.nome}!${c.premio>0?` +${fmt(c.premio)} 🍉`:""}`, "ach"))
      }
      return next
    })
  },[total,clicks,tGens,tUps,rebirths,pps,mel])

  const toast = (msg,type="norm")=>{
    const id=tid.current++
    setToasts(ts=>[...ts,{id,msg,type}])
    setTimeout(()=>setToasts(ts=>ts.filter(t=>t.id!==id)),4000)
  }

  const click = (e)=>{
    const g=pcRef.current
    setMel(m=>m+g); setTotal(t=>t+g); setClicks(c=>c+1)
    setWiggle(true); setTimeout(()=>setWiggle(false),130)
    const id=fid.current++
    const r=e.currentTarget.getBoundingClientRect()
    const x=(e.clientX-r.left)/r.width*100, y=(e.clientY-r.top)/r.height*100
    setFloats(f=>[...f,{id,x,y,v:g}])
    setTimeout(()=>setFloats(f=>f.filter(fl=>fl.id!==id)),900)
  }

  const buyGen = id=>{
    const g=gens[id], c=Math.floor(g.custo*(1-discount))
    if(mel<c) return
    setMel(m=>m-c)
    setGens(gs=>gs.map((gen,i)=>i===id?{...gen,qtd:gen.qtd+1,custo:Math.ceil(gen.custo*1.15)}:gen))
  }
  const buyUp = id=>{ const u=ups[id]; if(u.ok||mel<u.custo) return; setMel(m=>m-u.custo); setUps(us=>us.map((up,i)=>i===id?{...up,ok:true}:up)) }
  const buyRup = id=>{
    const r=rups[id]; if(r.ok||seeds<r.custo) return
    setSeeds(s=>s-r.custo); setRups(rs=>rs.map((ru,i)=>i===id?{...ru,ok:true}:ru))
    toast(`🌱 ${r.nome} ativado!`,"seed")
  }
  const sell = pct=>{ setMel(m=>{const q=Math.floor(m*pct); if(!q) return m; setCash(c=>c+q*0.3); return m-q}) }
  const doRebirth = ()=>{
    if(seedPrev<1){toast("❌ Colha mais melancias antes!"); setConfirm(false); return}
    setAnim(true)
    setTimeout(()=>{
      setSeedst:setSeeds(s=>s+seedPrev); setTotalSeeds(t=>t+seedPrev)
      setRebirths(r=>r+1); setMel(0); setTotal(0); setClicks(0)
      setGens(GENS.map(g=>({...g,qtd:0,custo:g.base}))); setUps(UPS.map(u=>({...u,ok:false})))
      setFloats([]); setConfirm(false); setAnim(false)
      toast(`🔄 Renasceu! +${seedPrev} 🌱`,"rebirth")
    },1200)
  }

  // fix: inline rebirth logic properly
  const doRebirthFinal = ()=>{
    if(seedPrev<1){toast("❌ Colha mais melancias antes!"); setConfirm(false); return}
    setAnim(true)
    setTimeout(()=>{
      setSeeds(s=>s+seedPrev)
      setTotalSeeds(t=>t+seedPrev)
      setRebirths(r=>r+1)
      setMel(0); setTotal(0); setClicks(0)
      setGens(GENS.map(g=>({...g,qtd:0,custo:g.base})))
      setUps(UPS.map(u=>({...u,ok:false})))
      setFloats([]); setConfirm(false); setAnim(false)
      toast(`🔄 Renasceu! +${seedPrev} 🌱`,"rebirth")
    },1200)
  }

  const NAV = [
    {id:"casa",label:"🍉 Casa"},{id:"upgrades",label:"⬆️ Upgrades"},
    {id:"gens",label:"⚙️ Geradores"},{id:"rebirth",label:"🔄 Rebirth",sp:true},
    {id:"vender",label:"💵 Vender"},{id:"conqs",label:"🏆 Conquistas"},
    {id:"stats",label:"📊 Stats"},
  ]

  const UPG_TIERS = [
    {label:"🤜 Básico",    ids:[0,1,2,3,4,5,6,7,8,9]},
    {label:"⚔️ Avançado",  ids:[10,11,12,13]},
    {label:"🌟 Lendário",  ids:[14,15]},
    {label:"💥 Divino",    ids:[16,17,18,19]},
  ]

  const CONQ_CATS = [
    {label:"🍉 Colheita",  tipos:["total","saldo"]},
    {label:"👆 Cliques",   tipos:["cliques"]},
    {label:"⚙️ Geradores", tipos:["geradores"]},
    {label:"⬆️ Upgrades",  tipos:["upgrades"]},
    {label:"📈 Produção",  tipos:["pps"]},
    {label:"🔄 Rebirth",   tipos:["rebirth"]},
  ]

  const conqOk = conqs.filter(c=>c.ok).length

  const screens = {
    casa: ()=>(
      <div onClick={click} className="relative flex flex-col items-center justify-center h-full overflow-hidden cursor-pointer select-none"
        style={{background:"radial-gradient(ellipse at 50% 60%,#0d2f0d 0%,#040e04 100%)"}}>
        {[{l:"8%",t:"8%",s:60,a:0},{l:"82%",t:"12%",s:45,a:1},{l:"5%",t:"70%",s:55,a:2},
          {l:"88%",t:"68%",s:50,a:0},{l:"45%",t:"5%",s:40,a:1},{l:"92%",t:"42%",s:35,a:2},
          {l:"3%",t:"45%",s:30,a:0},{l:"60%",t:"88%",s:45,a:1},{l:"25%",t:"82%",s:38,a:2}
        ].map((d,i)=>(
          <div key={i} className="absolute pointer-events-none opacity-[0.06]"
            style={{left:d.l,top:d.t,fontSize:d.s,animation:`wf${d.a} ${7+i*1.3}s ease-in-out infinite ${i*0.7}s`}}>🍉</div>
        ))}
        {rebirths>0&&<div className="absolute top-4 left-1/2 -translate-x-1/2 px-3 py-1 rounded-full text-xs font-bold z-10"
          style={{background:"rgba(200,120,0,0.3)",border:"1px solid #c87800",color:"#f0b040"}}>
          🔄 {rebirths}x Renascido · 🌱 {seeds} Sementes
        </div>}
        <div className="z-10 text-center mb-6">
          <div className="text-5xl font-black" style={{color:"#a3f0a3",textShadow:"0 0 30px rgba(100,220,100,0.4)",fontFamily:"Georgia,serif"}}>
            🍉 {fmt(mel)}
          </div>
          <div className="text-sm mt-1" style={{color:"#4a7a4a"}}>
            {pps>0?`+${fmt(pps)}/s · `:""}{`+${fmt(pc)}/clique`}
            {prodMult>1&&<span style={{color:"#c87800"}}> · {prodMult.toFixed(1)}x prod</span>}
          </div>
        </div>
        <div className="relative z-10">
          <div style={{fontSize:200,lineHeight:1,
            transform:wiggle?"scale(0.87) rotate(-6deg)":"scale(1)",
            transition:"transform 0.12s cubic-bezier(.36,.07,.19,.97)",
            filter:`drop-shadow(0 0 ${wiggle?50:25}px rgba(80,200,80,${wiggle?0.7:0.3}))`,userSelect:"none"}}>🍉</div>
          {floats.map(f=>(
            <div key={f.id} className="absolute pointer-events-none font-black"
              style={{left:`${f.x}%`,top:`${f.y}%`,transform:"translate(-50%,-50%)",color:"#a3f0a3",
                fontSize:22,textShadow:"0 0 14px rgba(120,255,120,0.9)",animation:"floatUp 0.9s ease-out forwards",zIndex:99}}>
              +{fmt(f.v)}</div>
          ))}
        </div>
        <div className="z-10 mt-4 text-xs" style={{color:"#2a4a2a"}}>Clique para colher melancias!</div>
      </div>
    ),

    upgrades: ()=>(
      <div className="h-full overflow-y-auto p-5" style={{background:"#060f06"}}>
        <div className="flex justify-between items-baseline mb-5">
          <h2 className="text-2xl font-black" style={{color:"#c8a840",fontFamily:"Georgia,serif"}}>UPGRADES DE CLIQUE</h2>
          <span className="text-xs" style={{color:"#4a7a4a"}}>{tUps}/{UPS.length} comprados</span>
        </div>
        {UPG_TIERS.map(tier=>(
          <div key={tier.label} className="mb-6">
            <div className="text-xs font-black mb-2 px-1" style={{color:"#8a6a20",textTransform:"uppercase",letterSpacing:"0.1em"}}>{tier.label}</div>
            <div className="space-y-2">
              {tier.ids.map(id=>{
                const u=ups[id], cb=!u.ok&&mel>=u.custo
                return (
                  <div key={id} onClick={()=>buyUp(id)}
                    className="flex items-center gap-4 p-4 rounded-xl cursor-pointer"
                    style={{background:u.ok?"rgba(20,70,20,0.4)":cb?"rgba(30,50,30,0.9)":"rgba(15,25,15,0.7)",
                      border:`1px solid ${u.ok?"#2a6a2a":cb?"#7a6020":"#1a3a1a"}`,opacity:u.ok?0.65:cb?1:0.45}}
                    onMouseEnter={e=>cb&&(e.currentTarget.style.borderColor="#c8a840")}
                    onMouseLeave={e=>e.currentTarget.style.borderColor=u.ok?"#2a6a2a":cb?"#7a6020":"#1a3a1a"}>
                    <div className="text-2xl w-10 h-10 flex items-center justify-center rounded-lg shrink-0" style={{background:"#0d1f0d"}}>{u.emoji}</div>
                    <div className="flex-1 min-w-0">
                      <div className="font-bold text-white truncate">{u.nome}</div>
                      <div className="text-xs truncate" style={{color:"#4a7a4a"}}>{u.desc}</div>
                      <div className="flex gap-4 mt-1 text-xs">
                        <span style={{color:"#60c060",fontWeight:600}}>+{fmt(u.bonus)}/clique</span>
                        {!u.ok&&<span style={{color:"#c8a840"}}>🍉 {fmt(u.custo)}</span>}
                        {u.ok&&<span style={{color:"#40b040",fontWeight:700}}>✓ COMPRADO</span>}
                      </div>
                    </div>
                  </div>
                )
              })}
            </div>
          </div>
        ))}
      </div>
    ),

    gens: ()=>(
      <div className="h-full overflow-y-auto p-5" style={{background:"#060f06"}}>
        <div className="flex justify-between items-baseline mb-1">
          <h2 className="text-2xl font-black" style={{color:"#c8a840",fontFamily:"Georgia,serif"}}>GERADORES</h2>
          <span style={{color:"#60c060",fontWeight:700,fontSize:13}}>+{fmt(pps)}/s</span>
        </div>
        {discount>0&&<p className="text-xs mb-1" style={{color:"#c87800"}}>🌱 Desconto ativo: -{Math.round(discount*100)}%!</p>}
        <p className="text-xs mb-4" style={{color:"#2a5a2a"}}>Total: {tGens} geradores · Mult: {prodMult.toFixed(1)}x</p>
        <div className="space-y-2">
          {gens.map(g=>{
            const c=Math.floor(g.custo*(1-discount)), cb=mel>=c
            return (
              <div key={g.id} onClick={()=>buyGen(g.id)}
                className="flex items-center gap-4 p-3 rounded-xl cursor-pointer"
                style={{background:cb?"rgba(25,45,25,0.95)":"rgba(12,20,12,0.8)",
                  border:`1px solid ${cb?"#7a6020":"#1a3a1a"}`,opacity:cb?1:0.45}}
                onMouseEnter={e=>cb&&(e.currentTarget.style.borderColor="#c8a840")}
                onMouseLeave={e=>e.currentTarget.style.borderColor=cb?"#7a6020":"#1a3a1a"}>
                <div className="w-12 h-12 flex items-center justify-center rounded-lg text-2xl shrink-0" style={{background:"#0d1f0d"}}>{g.emoji}</div>
                <div className="flex-1 min-w-0">
                  <div className="font-bold text-white">{g.nome}</div>
                  <div className="flex gap-4 mt-1 text-xs">
                    <span style={{color:"#5090c0"}}>+{fmt(Math.floor(g.pps*prodMult))}/s</span>
                    <span style={{color:"#c8a840"}}>🍉 {fmt(c)}</span>
                    {g.qtd>0&&<span style={{color:"#60c060"}}>×{g.qtd} (+{fmt(Math.floor(g.pps*g.qtd*prodMult))}/s)</span>}
                  </div>
                </div>
                <div className="flex flex-col items-center gap-1.5 shrink-0">
                  <div className="text-2xl font-black text-white w-10 text-center">{g.qtd}</div>
                  <div className="px-3 py-1 rounded-lg text-xs font-extrabold uppercase"
                    style={{background:cb?"#c0210d":"#2a3a2a",color:cb?"white":"#3a5a3a"}}>Comprar</div>
                </div>
              </div>
            )
          })}
        </div>
      </div>
    ),

    rebirth: ()=>(
      <div className="h-full overflow-y-auto p-5" style={{background:"#060f06"}}>
        <h2 className="text-2xl font-black mb-1" style={{color:"#f0a020",fontFamily:"Georgia,serif"}}>🔄 REBIRTH</h2>
        <p className="text-xs mb-5" style={{color:"#6a5a2a"}}>Recomece do zero e torne-se mais poderoso a cada renascimento.</p>

        <div className="rounded-2xl p-5 mb-5 border" style={{background:"rgba(40,25,0,0.6)",borderColor:"#6a4a00"}}>
          <div className="grid grid-cols-3 gap-3 mb-4">
            {[["🔄 Rebirths",rebirths],["🌱 Sementes",seeds],["🌱 Total",totalSeeds],
              ["📈 Prod Mult",prodMult.toFixed(1)+"x"],["👆 Click Mult",clickMult.toFixed(1)+"x"],["🎁 Ganho","+"+seedPrev]
            ].map(([l,v])=>(
              <div key={l} className="rounded-xl p-3" style={{background:"rgba(30,20,0,0.8)"}}>
                <div className="text-xs mb-1" style={{color:"#8a6a2a"}}>{l}</div>
                <div className="font-black text-lg" style={{color:"#f0c040"}}>{v}</div>
              </div>
            ))}
          </div>
          <div className="rounded-xl p-3 mb-4 text-xs" style={{background:"rgba(60,30,0,0.5)",border:"1px solid #5a3a00"}}>
            <span style={{color:"#8a4030"}}>⚠️ Reseta: melancias, geradores, upgrades de clique</span>
            <br/><span style={{color:"#60c060"}}>✅ Mantém: conquistas, sementes, upgrades de rebirth</span>
          </div>
          <div className="text-center mb-4">
            <span style={{color:"#a08030",fontSize:13}}>Você vai ganhar </span>
            <span className="text-2xl font-black" style={{color:"#f0c040"}}>🌱 {seedPrev}</span>
            <div className="text-xs mt-1" style={{color:"#6a5a2a"}}>Fórmula: √(total colhido / 1M) × {seedMult.toFixed(1)}x</div>
          </div>
          <button onClick={()=>seedPrev>=1&&setConfirm(true)}
            className="w-full py-4 rounded-xl font-black text-lg transition-all"
            style={{
              background:seedPrev>=1?"linear-gradient(135deg,#c87000,#a05000)":"#2a2010",
              color:seedPrev>=1?"white":"#4a3a20",border:`2px solid ${seedPrev>=1?"#f0a020":"#3a2a10"}`,
              cursor:seedPrev>=1?"pointer":"default",boxShadow:seedPrev>=1?"0 0 20px rgba(200,120,0,0.4)":"none"
            }}>🔄 RENASCER AGORA</button>
          {seedPrev<1&&<p className="text-center text-xs mt-2" style={{color:"#5a4a20"}}>Colha 1M melancias no total para renascer</p>}
        </div>

        <h3 className="text-lg font-black mb-3" style={{color:"#c8a840"}}>🌱 Upgrades de Rebirth</h3>
        <div className="space-y-2">
          {rups.map(r=>{
            const cb=!r.ok&&seeds>=r.custo
            return (
              <div key={r.id} onClick={()=>buyRup(r.id)}
                className="flex items-center gap-4 p-4 rounded-xl cursor-pointer"
                style={{background:r.ok?"rgba(20,60,10,0.5)":cb?"rgba(30,25,5,0.9)":"rgba(12,10,2,0.7)",
                  border:`1px solid ${r.ok?"#2a6a10":cb?"#8a6010":"#2a2a10"}`,opacity:r.ok?0.7:cb?1:0.45}}
                onMouseEnter={e=>cb&&(e.currentTarget.style.borderColor="#f0c040")}
                onMouseLeave={e=>e.currentTarget.style.borderColor=r.ok?"#2a6a10":cb?"#8a6010":"#2a2a10"}>
                <div className="text-2xl w-10 h-10 flex items-center justify-center rounded-lg shrink-0" style={{background:"#1a1500"}}>{r.emoji}</div>
                <div className="flex-1">
                  <div className="font-bold text-white">{r.nome}</div>
                  <div className="text-xs" style={{color:"#6a5a2a"}}>{r.desc}</div>
                  <div className="flex gap-3 mt-1 text-xs">
                    {!r.ok&&<span style={{color:"#f0c040"}}>🌱 {r.custo} Sementes</span>}
                    {r.ok&&<span style={{color:"#60c060",fontWeight:700}}>✓ ATIVO</span>}
                  </div>
                </div>
              </div>
            )
          })}
        </div>
      </div>
    ),

    vender: ()=>(
      <div className="p-6 overflow-y-auto h-full" style={{background:"#060f06"}}>
        <h2 className="text-2xl font-black mb-6" style={{color:"#c8a840",fontFamily:"Georgia,serif"}}>VENDER MELANCIAS</h2>
        <div className="rounded-2xl overflow-hidden mb-4 border" style={{borderColor:"#2a5a2a"}}>
          <div style={{background:"linear-gradient(135deg,#5a3800,#3a2200)",padding:"16px 20px"}}>
            <h3 className="text-white font-black text-lg">Mercado de Melancias</h3>
            <p style={{color:"#c8a840",fontSize:12,marginTop:2}}>Venda suas melancias por dinheiro!</p>
          </div>
          <div className="p-5 flex gap-10" style={{background:"#0d1f0d"}}>
            <div><div className="text-xs mb-1" style={{color:"#4a7a4a"}}>Melancias</div><div className="text-2xl font-black" style={{color:"#a3f0a3"}}>🍉 {fmt(mel)}</div></div>
            <div><div className="text-xs mb-1" style={{color:"#4a7a4a"}}>Dinheiro</div><div className="text-2xl font-black" style={{color:"#60d060"}}>R${cash.toFixed(2)}</div></div>
          </div>
        </div>
        <div className="rounded-xl p-4 mb-4 border" style={{background:"#0d1f0d",borderColor:"#1a3a1a"}}>
          <div className="text-sm text-white font-bold mb-1">Preço: R$0,30/melancia</div>
          <div className="text-sm">Valor total: <span className="font-black" style={{color:"#c8a840"}}>R${(mel*0.3).toFixed(2)}</span></div>
        </div>
        <div className="rounded-xl p-5 border" style={{background:"#0d1f0d",borderColor:"#1a3a1a"}}>
          <h3 className="text-white font-bold mb-4">Venda Manual</h3>
          <div className="grid grid-cols-3 gap-3">
            {[[0.10,"Vender 10%","#7a5800","#9a7200"],[0.50,"Vender 50%","#8a3600","#aa4800"],[1.00,"Vender Tudo","#1a6a1a","#2a8a2a"]].map(([p,l,bg,h])=>(
              <button key={p} onClick={()=>sell(p)} className="py-3 rounded-xl font-extrabold text-white text-sm"
                style={{background:bg}} onMouseEnter={e=>e.currentTarget.style.background=h}
                onMouseLeave={e=>e.currentTarget.style.background=bg}>{l}</button>
            ))}
          </div>
        </div>
      </div>
    ),

    conqs: ()=>(
      <div className="h-full overflow-y-auto p-5" style={{background:"#060f06"}}>
        <div className="flex justify-between items-baseline mb-2">
          <h2 className="text-2xl font-black" style={{color:"#c8a840",fontFamily:"Georgia,serif"}}>CONQUISTAS</h2>
          <span style={{color:"#c8a840",fontSize:13,fontWeight:700}}>{conqOk}/{CONQS.length}</span>
        </div>
        <div className="rounded-full mb-5 overflow-hidden" style={{height:6,background:"#1a2a1a"}}>
          <div className="h-full rounded-full" style={{width:`${(conqOk/CONQS.length)*100}%`,background:"linear-gradient(90deg,#40a040,#a8e040)",transition:"width 0.5s"}}/>
        </div>
        {CONQ_CATS.map(cat=>{
          const lista=conqs.filter(c=>cat.tipos.includes(c.tipo))
          return (
            <div key={cat.label} className="mb-5">
              <div className="text-xs font-black mb-2" style={{color:"#8a6a20",textTransform:"uppercase",letterSpacing:"0.1em"}}>
                {cat.label} — {lista.filter(c=>c.ok).length}/{lista.length}
              </div>
              <div className="space-y-2">
                {lista.map(c=>(
                  <div key={c.id} className="flex items-center gap-4 p-4 rounded-xl border"
                    style={{background:c.ok?"rgba(30,80,20,0.4)":"rgba(10,20,10,0.7)",borderColor:c.ok?"#3a7a2a":"#1a2a1a"}}>
                    <div className="text-3xl shrink-0">{c.ok?"🏆":"🔒"}</div>
                    <div className="flex-1">
                      <div className="font-bold" style={{color:c.ok?"#d4f080":"#4a6a4a"}}>{c.nome}</div>
                      <div className="text-xs" style={{color:"#2a5a2a"}}>{c.desc}</div>
                      <div className="text-xs mt-1">
                        {c.ok?<span style={{color:"#60c060",fontWeight:700}}>✅ Desbloqueada!</span>
                          :c.premio>0?<span style={{color:"#c8a840"}}>Recompensa: +{fmt(c.premio)} 🍉</span>
                          :<span style={{color:"#c87800"}}>🏅 Conquista Especial</span>}
                      </div>
                    </div>
                  </div>
                ))}
              </div>
            </div>
          )
        })}
      </div>
    ),

    stats: ()=>(
      <div className="h-full overflow-y-auto p-5" style={{background:"#060f06"}}>
        <h2 className="text-2xl font-black mb-5" style={{color:"#c8a840",fontFamily:"Georgia,serif"}}>ESTATÍSTICAS</h2>
        <div className="rounded-xl overflow-hidden border" style={{borderColor:"#1a3a1a"}}>
          {[
            ["Melancias atuais",  `🍉 ${fmt(mel)}`,                "#a3f0a3"],
            ["Total colhido",     `🍉 ${fmt(total)}`,              "#e8e8e8"],
            ["Dinheiro",          `R$${cash.toFixed(2)}`,          "#e8e8e8"],
            ["Por clique",        `+${fmt(pc)}`,                   "#e8e8e8"],
            ["Por segundo",       `+${fmt(pps)}`,                  "#e8e8e8"],
            ["Mult. produção",    `${prodMult.toFixed(2)}x`,        "#f0a020"],
            ["Mult. clique",      `${clickMult.toFixed(2)}x`,       "#f0a020"],
            ["Desconto geradores",`${Math.round(discount*100)}%`,  "#f0a020"],
            ["Cliques totais",    clicks.toLocaleString("pt-BR"),   "#e8e8e8"],
            ["Geradores",         tGens.toString(),                 "#e8e8e8"],
            ["Upgrades clique",   `${tUps}/${UPS.length}`,         "#e8e8e8"],
            ["Conquistas",        `${conqOk}/${CONQS.length}`,     "#e8e8e8"],
            ["Rebirths feitos",   rebirths.toString(),              "#f0a020"],
            ["Sementes atuais",   `🌱 ${seeds}`,                   "#f0a020"],
            ["Sementes totais",   `🌱 ${totalSeeds}`,              "#f0a020"],
            ["Seed Mult",         `${seedMult.toFixed(1)}x`,       "#f0a020"],
          ].map(([l,v,c],i)=>(
            <div key={i} className="flex justify-between items-center px-5 py-3 text-sm"
              style={{background:i%2===0?"#0d1f0d":"#0a170a",borderBottom:"1px solid #1a2a1a"}}>
              <span style={{color:"#4a7a4a"}}>{l}</span>
              <span style={{color:c,fontWeight:700}}>{v}</span>
            </div>
          ))}
        </div>
      </div>
    ),
  }

  return (
    <div className="flex h-screen overflow-hidden text-white" style={{fontFamily:"'Segoe UI',system-ui,sans-serif",background:"#040904"}}>

      {/* REBIRTH ANIM */}
      {anim&&<div className="fixed inset-0 z-[100] flex items-center justify-center" style={{background:"rgba(0,0,0,0.92)"}}>
        <div className="text-center">
          <div style={{fontSize:120,animation:"rSpin 1.2s ease-in-out",filter:"drop-shadow(0 0 60px rgba(255,160,0,0.8))"}}>🔄</div>
          <div className="text-3xl font-black mt-4" style={{color:"#f0c040"}}>RENASCENDO...</div>
        </div>
      </div>}

      {/* CONFIRM MODAL */}
      {confirm&&<div className="fixed inset-0 z-50 flex items-center justify-center" style={{background:"rgba(0,0,0,0.75)"}}>
        <div className="rounded-2xl p-8 max-w-sm w-full mx-4 border" style={{background:"#0d1a05",borderColor:"#c87800"}}>
          <div className="text-center mb-6">
            <div style={{fontSize:60}}>🔄</div>
            <h3 className="text-2xl font-black mt-2" style={{color:"#f0c040"}}>Confirmar Rebirth?</h3>
            <p className="mt-2 text-sm" style={{color:"#8a7030"}}>Você receberá <span className="font-black text-xl" style={{color:"#f0c040"}}>🌱 {seedPrev}</span></p>
            <p className="text-xs mt-1" style={{color:"#5a4020"}}>Melancias, geradores e upgrades serão resetados.</p>
          </div>
          <div className="flex gap-3">
            <button onClick={()=>setConfirm(false)} className="flex-1 py-3 rounded-xl font-bold"
              style={{background:"#1a2a1a",color:"#4a7a4a",border:"1px solid #2a4a2a"}}>Cancelar</button>
            <button onClick={doRebirthFinal} className="flex-1 py-3 rounded-xl font-black text-white"
              style={{background:"linear-gradient(135deg,#c87000,#a05000)",border:"2px solid #f0a020"}}>🔄 Renascer!</button>
          </div>
        </div>
      </div>}

      {/* TOASTS */}
      <div className="fixed top-4 right-4 z-40 flex flex-col gap-2 pointer-events-none">
        {toasts.map(t=>(
          <div key={t.id} className="px-4 py-3 rounded-xl text-sm font-bold shadow-2xl"
            style={{
              background:t.type==="rebirth"?"linear-gradient(135deg,#5a3000,#3a1a00)":t.type==="seed"?"linear-gradient(135deg,#1a3a00,#0a2000)":t.type==="ach"?"linear-gradient(135deg,#3a2a00,#2a1a00)":"linear-gradient(135deg,#5a4a00,#3a3000)",
              border:`1px solid ${t.type==="rebirth"?"#f0a020":t.type==="seed"?"#60c020":"#c8a840"}`,
              color:t.type==="rebirth"?"#f0c040":t.type==="seed"?"#a0e040":"#f0d070",
              animation:"slideIn 0.3s ease-out"
            }}>{t.msg}</div>
        ))}
      </div>

      {/* NAV */}
      <div className="flex flex-col py-4 gap-1 px-2 shrink-0" style={{width:160,background:"#061206",borderRight:"1px solid #1a3a1a"}}>
        <div className="text-center py-3 mb-2 select-none">
          <div style={{fontSize:36,filter:"drop-shadow(0 0 15px rgba(80,200,80,0.5))"}}>🍉</div>
          <div style={{color:"#60c060",fontWeight:900,fontSize:13,fontFamily:"Georgia,serif",lineHeight:1.2}}>Melancia<br/>Clicker</div>
          {rebirths>0&&<div style={{color:"#f0a020",fontSize:10,fontWeight:800,marginTop:2}}>🔄 {rebirths}x</div>}
        </div>
        {NAV.map(n=>(
          <button key={n.id} onClick={()=>setTab(n.id)}
            className="py-2.5 px-3 rounded-xl text-left text-sm font-bold transition-all"
            style={{
              background:tab===n.id?n.sp?"#c87000":"#c8a840":"transparent",
              color:tab===n.id?"#1a0e00":n.sp?"#c87000":"#5a8a5a",
              border:tab===n.id?`1px solid ${n.sp?"#f0b020":"#e0c050"}`:"1px solid transparent"
            }}
            onMouseEnter={e=>tab!==n.id&&(e.currentTarget.style.background="#0d2f0d")}
            onMouseLeave={e=>tab!==n.id&&(e.currentTarget.style.background="transparent")}
          >{n.label}</button>
        ))}
        <div className="mt-auto mx-1 p-3 rounded-xl select-none" style={{background:"#0a1f0a",border:"1px solid #1a3a1a"}}>
          <div style={{color:"#80d080",fontWeight:800,fontSize:12}}>🍉 {fmt(mel)}</div>
          <div style={{color:"#3a6a3a",fontSize:10}}>+{fmt(pps)}/s · +{fmt(pc)}/clique</div>
          {seeds>0&&<div style={{color:"#c87800",fontSize:10,marginTop:2}}>🌱 {seeds} sementes</div>}
        </div>
      </div>

      {/* CONTENT */}
      <div className="flex-1 overflow-hidden">{(screens[tab]||screens.casa)()}</div>

      {/* RIGHT STATS */}
      <div className="p-4 text-sm overflow-y-auto shrink-0" style={{width:188,background:"#061206",borderLeft:"1px solid #1a3a1a"}}>
        <div className="font-black mb-4 text-base" style={{color:"#c8a840",fontFamily:"Georgia,serif"}}>📊 STATS</div>
        <SS title="Saldo"><SR l="Melancias" v={`🍉 ${fmt(mel)}`}/><SR l="Dinheiro" v={`R$${cash.toFixed(2)}`}/></SS>
        <SS title="Produção"><SR l="Por Clique" v={`+${fmt(pc)}`}/><SR l="Por Segundo" v={`+${fmt(pps)}`}/><SR l="Prod Mult" v={`${prodMult.toFixed(1)}x`} sp/><SR l="Click Mult" v={`${clickMult.toFixed(1)}x`} sp/></SS>
        <SS title="Total"><SR l="Colhido" v={fmt(total)}/><SR l="Cliques" v={clicks.toLocaleString("pt-BR")}/></SS>
        <SS title="Rebirth"><SR l="Vezes" v={rebirths.toString()} sp/><SR l="Sementes" v={`🌱 ${seeds}`} sp/><SR l="Projetado" v={`+${seedPrev}`} sp/></SS>
        <SS title="Progresso">
          <SR l="Geradores" v={tGens.toString()}/><SR l="Upgrades" v={`${tUps}/${UPS.length}`}/>
          <SR l="Conquistas" v={`${conqOk}/${CONQS.length}`}/>
        </SS>
        <div className="rounded-full mt-2 overflow-hidden" style={{height:4,background:"#1a2a1a"}}>
          <div style={{width:`${(conqOk/CONQS.length)*100}%`,height:"100%",background:"linear-gradient(90deg,#40a040,#a8e040)",transition:"width 0.5s"}}/>
        </div>
        <div className="text-right text-xs mt-1" style={{color:"#4a7a4a"}}>{conqOk}/{CONQS.length}</div>
      </div>

      <style>{`
        @keyframes floatUp{0%{opacity:1;transform:translate(-50%,-50%) scale(1)}100%{opacity:0;transform:translate(-50%,-130%) scale(1.5)}}
        @keyframes wf0{0%,100%{transform:translateY(0) rotate(0deg)}50%{transform:translateY(-18px) rotate(6deg)}}
        @keyframes wf1{0%,100%{transform:translateY(0) rotate(0deg)}50%{transform:translateY(-24px) rotate(-8deg)}}
        @keyframes wf2{0%,100%{transform:translateY(0) rotate(0deg)}50%{transform:translateY(-14px) rotate(4deg)}}
        @keyframes slideIn{from{opacity:0;transform:translateX(20px)}to{opacity:1;transform:translateX(0)}}
        @keyframes rSpin{0%{transform:scale(0) rotate(-180deg)}60%{transform:scale(1.3) rotate(20deg)}100%{transform:scale(1) rotate(0deg)}}
        ::-webkit-scrollbar{width:4px}::-webkit-scrollbar-track{background:#060f06}::-webkit-scrollbar-thumb{background:#2a5a2a;border-radius:2px}
      `}</style>
    </div>
  )
}
