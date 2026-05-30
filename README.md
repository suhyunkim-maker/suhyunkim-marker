import { useState, useEffect, useRef } from "react";

const TYPING_LINES = [
  "AI | Media | Data Explorer 🚀",
  "Journalist × Product Manager × Data Analyst",
  "Building AI-powered Media Services 🎯",
  "Let's build something amazing together!",
];

const SKILLS_TECH = [
  { name: "🐍 Python", pct: 80, color: "from-blue-500 to-cyan-400" },
  { name: "📊 Data Analysis (Pandas / SQL)", pct: 72, color: "from-purple-700 to-purple-400" },
  { name: "✨ Generative AI (Gemini / Imagen)", pct: 75, color: "from-blue-400 to-sky-300" },
  { name: "🔥 Flask / Backend", pct: 65, color: "from-gray-500 to-gray-400" },
  { name: "🗄️ PostgreSQL / Supabase", pct: 68, color: "from-blue-700 to-blue-400" },
];

const SKILLS_DOMAIN = [
  { name: "📰 News Writing & Journalism", pct: 90, color: "from-rose-500 to-pink-400" },
  { name: "📋 Service Planning & Strategy", pct: 82, color: "from-violet-600 to-purple-400" },
  { name: "🚀 Product Management (PM)", pct: 78, color: "from-indigo-600 to-blue-400" },
];

const SNS = [
  { icon: "🐱", label: "GitHub", bg: "#24292e", border: "#555" },
  { icon: "✍️", label: "Velog", bg: "#20c997" },
  { icon: "📝", label: "Portfolio", bg: "#000", border: "#555" },
  { icon: "📧", label: "Email", bg: "#EA4335" },
  { icon: "💼", label: "LinkedIn", bg: "#0077B5" },
];

const TOOL_GROUPS = [
  {
    label: "🤖 AI Assistants",
    badges: [
      { text: "Claude", bg: "#CC785C", icon: "🧠" },
      { text: "Perplexity", bg: "#1DB9C3", icon: "🔍" },
      { text: "NotebookLM", bg: "#4285F4", icon: "📓" },
    ],
  },
  {
    label: "🎨 AI Creative Tools",
    badges: [
      { text: "Suno", bg: "#7C3AED", icon: "🎵" },
      { text: "CapCut", bg: "#000000", border: "#555", icon: "🎬" },
      { text: "ElevenLabs", bg: "#1A1A2E", border: "#555", icon: "🎙️" },
    ],
  },
  {
    label: "💻 Dev Tools",
    badges: [
      { text: "VSCode", bg: "#007ACC", icon: "💙" },
      { text: "Jupyter Notebook", bg: "#F37726", icon: "📔" },
      { text: "N8N", bg: "#EA4B71", icon: "⚡" },
      { text: "브루 (Brew)", bg: "#FBB040", dark: true, icon: "🍺" },
    ],
  },
  {
    label: "📊 Data & Analysis",
    badges: [
      { text: "Excel + SQL함수", bg: "#217346", icon: "📈" },
      { text: "Python", bg: "#3776AB", icon: "🐍" },
      { text: "Pandas", bg: "#150458", icon: "🐼" },
      { text: "PostgreSQL", bg: "#336791", icon: "🐘" },
    ],
  },
  {
    label: "🔧 Backend & DB",
    badges: [
      { text: "Flask", bg: "#333", icon: "🔥" },
      { text: "Supabase", bg: "#3ECF8E", dark: true, icon: "⚡" },
      { text: "Git", bg: "#F05032", icon: "🔴" },
      { text: "Notion", bg: "#000", border: "#555", icon: "📝" },
    ],
  },
];

function TypingText() {
  const [text, setText] = useState("");
  const [lineIdx, setLineIdx] = useState(0);
  const [deleting, setDeleting] = useState(false);
  useEffect(() => {
    const cur = TYPING_LINES[lineIdx];
    const timeout = setTimeout(() => {
      if (!deleting) {
        setText(cur.slice(0, text.length + 1));
        if (text.length + 1 === cur.length) setTimeout(() => setDeleting(true), 1600);
      } else {
        setText(cur.slice(0, text.length - 1));
        if (text.length - 1 === 0) {
          setDeleting(false);
          setLineIdx((i) => (i + 1) % TYPING_LINES.length);
        }
      }
    }, deleting ? 40 : 70);
    return () => clearTimeout(timeout);
  }, [text, deleting, lineIdx]);

  return (
    <span style={{ fontFamily: "monospace", color: "#a78bfa", fontWeight: 600, fontSize: "1.05rem" }}>
      {text}
      <span style={{ borderRight: "2px solid #a78bfa", animation: "blink 0.75s step-end infinite", marginLeft: 1 }} />
    </span>
  );
}

function SkillBar({ name, pct, color, visible }) {
  const gradients = {
    "from-blue-500 to-cyan-400": "linear-gradient(90deg,#3b82f6,#22d3ee)",
    "from-purple-700 to-purple-400": "linear-gradient(90deg,#7e22ce,#c084fc)",
    "from-blue-400 to-sky-300": "linear-gradient(90deg,#60a5fa,#7dd3fc)",
    "from-gray-500 to-gray-400": "linear-gradient(90deg,#6b7280,#9ca3af)",
    "from-blue-700 to-blue-400": "linear-gradient(90deg,#1d4ed8,#60a5fa)",
    "from-rose-500 to-pink-400": "linear-gradient(90deg,#f43f5e,#f472b6)",
    "from-violet-600 to-purple-400": "linear-gradient(90deg,#7c3aed,#c084fc)",
    "from-indigo-600 to-blue-400": "linear-gradient(90deg,#4f46e5,#60a5fa)",
  };
  return (
    <div style={{ marginBottom: 14 }}>
      <div style={{ display: "flex", justifyContent: "space-between", marginBottom: 5 }}>
        <span style={{ fontSize: "0.85rem", color: "#e6edf3", fontWeight: 600 }}>{name}</span>
        <span style={{ fontSize: "0.78rem", color: "#8b949e", fontFamily: "monospace" }}>{pct}%</span>
      </div>
      <div style={{ width: "100%", height: 10, background: "#21262d", borderRadius: 99, overflow: "hidden" }}>
        <div style={{
          height: "100%", borderRadius: 99,
          background: gradients[color],
          width: visible ? `${pct}%` : "0%",
          transition: "width 1.3s cubic-bezier(.4,0,.2,1)",
        }} />
      </div>
    </div>
  );
}

function SectionTitle({ children }) {
  return (
    <h2 style={{ fontSize: "1.05rem", color: "#e6edf3", margin: "30px 0 14px", borderBottom: "1px solid #21262d", paddingBottom: 8 }}>
      {children}
    </h2>
  );
}

function BadgeChip({ text, bg, border, dark, icon }) {
  return (
    <span style={{
      display: "inline-flex", alignItems: "center", gap: 5,
      padding: "6px 13px", borderRadius: 7,
      background: bg,
      border: border ? `1px solid ${border}` : "none",
      color: dark ? "#000" : "#fff",
      fontSize: "0.78rem", fontWeight: 600,
    }}>
      {icon && <span style={{ fontSize: "0.85rem" }}>{icon}</span>}
      {text}
    </span>
  );
}

export default function GitHubProfile() {
  const [skillsVisible, setSkillsVisible] = useState(false);
  const skillsRef = useRef(null);

  useEffect(() => {
    const obs = new IntersectionObserver(([e]) => { if (e.isIntersecting) setSkillsVisible(true); }, { threshold: 0.1 });
    if (skillsRef.current) obs.observe(skillsRef.current);
    return () => obs.disconnect();
  }, []);

  return (
    <div style={{ background: "#0d1117", minHeight: "100vh", color: "#c9d1d9", fontFamily: "sans-serif" }}>
      <style>{`
        @keyframes blink { 50% { opacity: 0 } }
        .sns-btn { cursor: pointer; transition: all .15s; }
        .sns-btn:hover { opacity: 0.82; transform: translateY(-2px); }
      `}</style>

      {/* ── HEADER ── */}
      <div style={{
        width: "100%", height: 190,
        background: "linear-gradient(135deg,#1a1a2e 0%,#16213e 25%,#0f3460 50%,#533483 75%,#e94560 100%)",
        display: "flex", flexDirection: "column", alignItems: "center", justifyContent: "center", gap: 8,
        position: "relative", overflow: "hidden",
      }}>
        <div style={{ fontFamily: "monospace", fontSize: "2.1rem", fontWeight: 700, color: "#fff", textShadow: "0 2px 12px rgba(0,0,0,0.4)", letterSpacing: 1 }}>
          suhyun's Github
        </div>
        <div style={{ fontSize: "0.9rem", color: "rgba(255,255,255,0.82)", letterSpacing: 4 }}>AI | Media | Data</div>
        <svg viewBox="0 0 1200 60" style={{ position: "absolute", bottom: -1, left: 0, width: "100%" }} preserveAspectRatio="none">
          <path d="M0,30 C200,60 400,0 600,30 C800,60 1000,0 1200,30 L1200,60 L0,60 Z" fill="#0d1117" />
        </svg>
      </div>

      <div style={{ maxWidth: 820, margin: "0 auto", padding: "20px 24px 0" }}>

        {/* TYPING */}
        <div style={{ textAlign: "center", margin: "18px 0 26px" }}>
          <TypingText />
        </div>

        {/* ABOUT ME */}
        <SectionTitle>🙋‍♀️ About Me</SectionTitle>
        <div style={{ background: "#161b22", border: "1px solid #30363d", borderRadius: 10, padding: "16px 20px", fontFamily: "monospace", fontSize: "0.82rem", lineHeight: 2, overflowX: "auto" }}>
          <span style={{ color: "#ffa657" }}>suhyun</span> <span style={{ color: "#79c0ff" }}>=</span> {"{"}<br />
          &nbsp;&nbsp;<span style={{ color: "#a5d6ff" }}>"role"</span> : [<span style={{ color: "#ffa657" }}>"Journalist"</span>, <span style={{ color: "#ffa657" }}>"Planner"</span>, <span style={{ color: "#ffa657" }}>"PM"</span>],<br />
          &nbsp;&nbsp;<span style={{ color: "#a5d6ff" }}>"AI_tools"</span> : [<span style={{ color: "#ffa657" }}>"Claude"</span>, <span style={{ color: "#ffa657" }}>"Perplexity"</span>, <span style={{ color: "#ffa657" }}>"Suno"</span>, <span style={{ color: "#ffa657" }}>"ElevenLabs"</span>, <span style={{ color: "#ffa657" }}>"N8N"</span>],<br />
          &nbsp;&nbsp;<span style={{ color: "#a5d6ff" }}>"passion"</span> : <span style={{ color: "#ffa657" }}>"AI × Media Convergence"</span>,<br />
          &nbsp;&nbsp;<span style={{ color: "#a5d6ff" }}>"goal"</span>    : <span style={{ color: "#ffa657" }}>"AI 시대를 선도하는 미디어 전문가"</span><br />
          {"}"}
        </div>

        {/* SKILL BARS */}
        <SectionTitle>📈 Skill Level</SectionTitle>
        <div ref={skillsRef}>
          <div style={{ fontSize: "0.76rem", color: "#8b949e", fontWeight: 700, marginBottom: 12, letterSpacing: 1 }}>💻 TECHNICAL</div>
          {SKILLS_TECH.map((s) => <SkillBar key={s.name} {...s} visible={skillsVisible} />)}
          <div style={{ fontSize: "0.76rem", color: "#8b949e", fontWeight: 700, margin: "20px 0 12px", letterSpacing: 1 }}>🎯 DOMAIN</div>
          {SKILLS_DOMAIN.map((s) => <SkillBar key={s.name} {...s} visible={skillsVisible} />)}
        </div>

        {/* TOOL STACK */}
        <SectionTitle>🛠️ Tech & Tool Stack</SectionTitle>
        {TOOL_GROUPS.map((g) => (
          <div key={g.label} style={{ marginBottom: 18 }}>
            <div style={{ fontSize: "0.76rem", color: "#8b949e", fontWeight: 700, marginBottom: 8, letterSpacing: 0.5 }}>{g.label}</div>
            <div style={{ display: "flex", flexWrap: "wrap", gap: 8 }}>
              {g.badges.map((b) => <BadgeChip key={b.text} {...b} />)}
            </div>
          </div>
        ))}

        {/* SNS */}
        <SectionTitle>📬 Contact & Links</SectionTitle>
        <div style={{ display: "flex", flexWrap: "wrap", gap: 10, marginBottom: 8 }}>
          {SNS.map((s) => (
            <button key={s.label} className="sns-btn" style={{
              display: "inline-flex", alignItems: "center", gap: 7,
              padding: "9px 18px", borderRadius: 8,
              background: s.bg, border: s.border ? `1px solid ${s.border}` : "none",
              color: "#fff", fontWeight: 600, fontSize: "0.85rem",
            }}>
              <span>{s.icon}</span>{s.label}
            </button>
          ))}
        </div>

        {/* PROJECT */}
        <SectionTitle>🌟 Featured Project</SectionTitle>
        <div style={{ background: "#161b22", border: "1px solid #30363d", borderRadius: 12, padding: 22 }}>
          <div style={{ color: "#e6edf3", fontWeight: 700, fontSize: "1rem", marginBottom: 8 }}>📰 AI 자동 카드뉴스 생성 서비스</div>
          <div style={{ color: "#8b949e", fontSize: "0.86rem", lineHeight: 1.7, marginBottom: 14 }}>
            Google Gemini &amp; Imagen API를 활용한 뉴스 자동 카드뉴스 변환 서비스
          </div>
          <div style={{ display: "flex", flexWrap: "wrap", gap: 6, marginBottom: 12 }}>
            {[["Flask", "#333"], ["Gemini API", "#4285F4"], ["Supabase", "#3ECF8E"], ["6인 팀프로젝트", "#FF6B6B"]].map(([t, bg]) => (
              <span key={t} style={{ padding: "4px 10px", borderRadius: 4, background: bg, color: bg === "#3ECF8E" ? "#000" : "#fff", fontSize: "0.74rem", fontWeight: 600 }}>{t}</span>
            ))}
          </div>
          <div style={{ color: "#3fb950", fontSize: "0.84rem" }}>
            ✅ 뉴스 트렌드 대시보드 &nbsp;|&nbsp; ✅ 이미지 자동 생성 &nbsp;|&nbsp; ✅ 기획~배포 전과정
          </div>
        </div>

        <div style={{ height: 44 }} />
      </div>

      {/* FOOTER WAVE */}
      <div style={{ width: "100%", height: 100, background: "linear-gradient(135deg,#e94560 0%,#533483 35%,#0f3460 70%,#1a1a2e 100%)", position: "relative", overflow: "hidden" }}>
        <svg viewBox="0 0 1200 60" style={{ position: "absolute", top: -1, left: 0, width: "100%" }} preserveAspectRatio="none">
          <path d="M0,30 C200,0 400,60 600,30 C800,0 1000,60 1200,30 L1200,0 L0,0 Z" fill="#0d1117" />
        </svg>
      </div>
    </div>
  );
}
