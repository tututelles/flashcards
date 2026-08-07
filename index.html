import { useState, useEffect, useRef, useCallback } from "react";
import { RotateCcw, Stethoscope, FlaskConical, ClipboardList, ChevronDown, Check, AlertCircle } from "lucide-react";

/* ---------- constants ---------- */
const ESPECIALIDADES = [
  "Clínica geral",
  "Cardiologia",
  "Pneumologia",
  "Gastroenterologia",
  "Neurologia",
  "Nefrologia",
  "Endocrinologia",
  "Infectologia",
];

const NIVEIS = [
  { id: "estudante", label: "Estudante", desc: "Apresentação clássica, um diagnóstico predominante" },
  { id: "interno", label: "Interno", desc: "Apresentação moderada, exige diagnóstico diferencial" },
  { id: "residente", label: "Residente", desc: "Caso atípico ou com comorbidades, raciocínio em múltiplas etapas" },
];

const LOADING_MESSAGES = [
  "Montando a história clínica…",
  "Definindo os achados do exame físico…",
  "Selecionando exames complementares…",
  "Revisando o caso…",
];

const STORAGE_KEY = "diferencial:stats";

/* ---------- small editorial divider (replaces literal border lines) ---------- */
function Ruler() {
  return (
    <div className="ruler" aria-hidden="true">
      <div className="ruler-line" />
      <div className="ruler-ticks" />
    </div>
  );
}

/* ---------- API call ---------- */
async function gerarCaso(especialidade, nivel) {
  const nivelInfo = NIVEIS.find((n) => n.id === nivel);
  const system = `Você é um elaborador de casos clínicos para treino de raciocínio diagnóstico de estudantes e residentes de medicina no Brasil.
Responda SOMENTE com um objeto JSON válido, sem markdown, sem crases, sem texto fora do JSON, no seguinte formato exato:
{
  "identificacao": "string curta: sexo, idade, contexto (1 frase)",
  "queixaPrincipal": "string curta, entre aspas, como o paciente diria",
  "hda": "história da doença atual, 2-4 frases",
  "antecedentes": "antecedentes pessoais e familiares relevantes, 1-3 frases",
  "exameFisico": "achados do exame físico incluindo sinais vitais, 3-5 frases ou itens separados por ponto",
  "examesComplementares": "resultados de exames laboratoriais e/ou de imagem relevantes, 3-5 itens",
  "diagnostico": "diagnóstico principal correto, curto",
  "raciocinio": "explicação do raciocínio clínico que leva ao diagnóstico, 80-140 palavras, tom didático",
  "pontosChave": ["ponto de ensino 1", "ponto de ensino 2", "ponto de ensino 3"]
}
O caso deve ser clinicamente plausível e coerente com o nível pedido. Varie sexo, idade e contexto a cada geração. Escreva tudo em português do Brasil.`;

  const user = `Gere um caso clínico novo de ${especialidade}, nível "${nivelInfo.label}" (${nivelInfo.desc}).`;

  const response = await fetch("https://api.anthropic.com/v1/messages", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      model: "claude-sonnet-4-6",
      max_tokens: 1000,
      system,
      messages: [{ role: "user", content: user }],
    }),
  });

  if (!response.ok) throw new Error("Falha na resposta da API");
  const data = await response.json();
  const text = (data.content || []).map((b) => b.text || "").join("\n");
  const clean = text.replace(/```json|```/g, "").trim();
  const parsed = JSON.parse(clean);
  return parsed;
}

/* ---------- main app ---------- */
export default function Diferencial() {
  const [stage, setStage] = useState("setup");
  const [especialidade, setEspecialidade] = useState(ESPECIALIDADES[0]);
  const [nivel, setNivel] = useState("estudante");
  const [caso, setCaso] = useState(null);
  const [revealed, setRevealed] = useState({ hist: false, exame: false, exames: false });
  const [hipotese, setHipotese] = useState("");
  const [loadingMsgIdx, setLoadingMsgIdx] = useState(0);
  const [caseNumber, setCaseNumber] = useState(1);
  const [stats, setStats] = useState({ casesCompleted: 0 });
  const [elapsed, setElapsed] = useState(0);
  const [mounted, setMounted] = useState(false);
  const timerRef = useRef(null);
  const loadingIntervalRef = useRef(null);

  const nivelIndex = NIVEIS.findIndex((n) => n.id === nivel);

  useEffect(() => {
    setMounted(true);
    (async () => {
      try {
        const result = await window.storage.get(STORAGE_KEY, false);
        if (result && result.value) {
          const parsed = JSON.parse(result.value);
          setStats(parsed);
          setCaseNumber((parsed.casesCompleted || 0) + 1);
        }
      } catch (e) {}
    })();
  }, []);

  useEffect(() => {
    if (stage === "case") {
      setElapsed(0);
      timerRef.current = setInterval(() => setElapsed((e) => e + 1), 1000);
      return () => clearInterval(timerRef.current);
    } else {
      clearInterval(timerRef.current);
    }
  }, [stage]);

  const persistStats = useCallback(async (next) => {
    try {
      await window.storage.set(STORAGE_KEY, JSON.stringify(next), false);
    } catch (e) {}
  }, []);

  const handleGerar = useCallback(async () => {
    setStage("loading");
    setLoadingMsgIdx(0);
    loadingIntervalRef.current = setInterval(() => {
      setLoadingMsgIdx((i) => (i + 1) % LOADING_MESSAGES.length);
    }, 1400);
    try {
      const result = await gerarCaso(especialidade, nivel);
      clearInterval(loadingIntervalRef.current);
      setCaso(result);
      setRevealed({ hist: false, exame: false, exames: false });
      setHipotese("");
      setStage("case");
    } catch (e) {
      clearInterval(loadingIntervalRef.current);
      setStage("error");
    }
  }, [especialidade, nivel]);

  const handleConfirmar = useCallback(() => {
    clearInterval(timerRef.current);
    setStage("discussion");
    const next = { casesCompleted: (stats.casesCompleted || 0) + 1 };
    setStats(next);
    persistStats(next);
  }, [stats, persistStats]);

  const handleNovoCaso = useCallback(() => {
    setCaseNumber((n) => n + 1);
    setStage("setup");
  }, []);

  const mm = String(Math.floor(elapsed / 60)).padStart(2, "0");
  const ss = String(elapsed % 60).padStart(2, "0");

  return (
    <div className={`diferencial-app ${mounted ? "is-mounted" : ""}`}>
      <style>{`
        .diferencial-app {
          --ink: #0B121C;
          --ink-2: #0F1A26;
          --panel: rgba(24, 36, 49, 0.6);
          --panel-strong: rgba(28, 42, 57, 0.76);
          --line: rgba(255,255,255,0.08);
          --line-bright: rgba(255,255,255,0.15);
          --pulse: #46E0B0;
          --pulse-ink: #0E3A2C;
          --pulse-dim: rgba(70, 224, 176, 0.28);
          --critical: #FF6B5E;
          --paper: #F1F4F6;
          --vapor: #93A1AE;
          --vapor-dim: #5C6771;

          font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", system-ui, sans-serif;
          background: linear-gradient(180deg, var(--ink-2), var(--ink) 60%);
          color: var(--paper);
          min-height: 100%;
          border-radius: 22px;
          border: 1px solid var(--line);
          box-shadow: 0 40px 100px -30px rgba(0,0,0,0.7), inset 0 1px 0 rgba(255,255,255,0.05);
          padding: 0;
          position: relative;
          overflow: hidden;
          opacity: 0;
          transform: translateY(8px) scale(0.995);
          transition: opacity 600ms cubic-bezier(0.22,1,0.36,1), transform 600ms cubic-bezier(0.22,1,0.36,1);
        }
        .diferencial-app.is-mounted { opacity: 1; transform: translateY(0) scale(1); }
        .diferencial-app * { box-sizing: border-box; }

        /* faint engineered grid, fading toward the bottom — a nod to chart paper, held very quiet */
        .diferencial-app::before {
          content: ""; position: absolute; inset: 0; pointer-events: none; z-index: 0;
          background-image:
            linear-gradient(rgba(255,255,255,0.025) 1px, transparent 1px),
            linear-gradient(90deg, rgba(255,255,255,0.025) 1px, transparent 1px);
          background-size: 28px 28px;
          -webkit-mask-image: radial-gradient(ellipse 85% 55% at 50% 0%, black, transparent 72%);
          mask-image: radial-gradient(ellipse 85% 55% at 50% 0%, black, transparent 72%);
        }

        .display-face { font-family: Fraunces, "Iowan Old Style", Georgia, serif; font-optical-sizing: auto; }
        .mono-face { font-family: "IBM Plex Mono", ui-monospace, SFMono-Regular, monospace; }

        .chrome {
          display: flex; align-items: center; justify-content: space-between;
          padding: 22px 30px 18px; position: relative; z-index: 2;
          border-bottom: 1px solid var(--line);
        }
        .brand { display: flex; align-items: center; gap: 12px; }
        .brand-mark {
          width: 32px; height: 32px; border-radius: 50%;
          background: var(--pulse); color: var(--pulse-ink);
          display: flex; align-items: center; justify-content: center;
          font-family: Fraunces, Georgia, serif; font-style: italic; font-weight: 600; font-size: 16px;
          box-shadow: inset 0 0 0 1px rgba(255,255,255,0.25), 0 2px 8px rgba(0,0,0,0.3);
        }
        .brand-text { display: flex; flex-direction: column; line-height: 1.15; }
        .brand-name { font-size: 18px; font-weight: 500; letter-spacing: -0.01em; }
        .brand-sub { font-family: "IBM Plex Mono", ui-monospace, monospace; font-size: 10.5px; letter-spacing: 0.1em; text-transform: uppercase; color: var(--vapor-dim); }

        .stats-chip {
          font-family: "IBM Plex Mono", ui-monospace, monospace; font-size: 12px; color: var(--vapor);
          border: 1px solid var(--line); padding: 7px 14px; border-radius: 999px;
          display: flex; align-items: center; gap: 9px;
        }
        .stats-chip::before { content: ""; width: 5px; height: 5px; border-radius: 50%; background: var(--pulse); }

        .stage { padding: 10px 30px 36px; position: relative; z-index: 2; }
        .stage-panel { animation: stage-in 520ms cubic-bezier(0.22,1,0.36,1); }
        @keyframes stage-in { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* editorial ruler divider — replaces plain hairline rules throughout */
        .ruler { margin: 16px 0 14px; }
        .ruler-line { height: 1px; background: var(--line); }
        .ruler-ticks { height: 4px; margin-top: 2px; background-image: repeating-linear-gradient(90deg, var(--line) 0 1px, transparent 1px 13px); }

        /* setup */
        .setup-card { max-width: 640px; margin: 30px auto 0; }
        .eyebrow {
          font-family: "IBM Plex Mono", ui-monospace, monospace; font-size: 11px; letter-spacing: 0.12em; text-transform: uppercase;
          color: var(--vapor); margin: 0 0 14px; display: flex; align-items: center; gap: 9px;
        }
        .eyebrow::before { content: ""; width: 16px; height: 1px; background: var(--pulse); }
        .setup-title { font-size: clamp(30px, 4.4vw, 44px); line-height: 1.06; letter-spacing: -0.025em; font-weight: 500; margin: 0 0 12px; }
        .setup-sub { color: var(--vapor); font-size: 16px; line-height: 1.6; margin: 0 0 34px; max-width: 48ch; }
        .field-group { margin-bottom: 26px; }
        .field-label { font-family: "IBM Plex Mono", ui-monospace, monospace; font-size: 11px; letter-spacing: 0.09em; text-transform: uppercase; color: var(--vapor); display: block; margin-bottom: 12px; }

        .chip-row { display: flex; flex-wrap: wrap; gap: 9px; }
        .chip {
          border: 1px solid var(--line); background: transparent; color: var(--paper);
          padding: 10px 16px; border-radius: 10px; font-size: 14px; cursor: pointer; font-family: inherit;
          transition: transform 130ms cubic-bezier(0.22,1,0.36,1), background 180ms ease, border-color 180ms ease;
        }
        .chip:hover { border-color: var(--line-bright); }
        .chip:active { transform: scale(0.96); }
        .chip.selected {
          background: var(--pulse); color: var(--pulse-ink); border-color: var(--pulse); font-weight: 600;
        }

        /* segmented control (nivel) */
        .segmented { position: relative; display: flex; background: var(--ink-2); border: 1px solid var(--line); border-radius: 12px; padding: 4px; }
        .segmented-indicator {
          position: absolute; top: 4px; bottom: 4px; left: 4px; width: calc((100% - 8px) / 3); border-radius: 9px;
          background: var(--pulse);
          box-shadow: inset 0 1px 0 rgba(255,255,255,0.3);
          transition: transform 440ms cubic-bezier(0.22,1,0.36,1); z-index: 0;
        }
        .segmented-btn {
          position: relative; z-index: 1; flex: 1; background: transparent; border: none; padding: 11px 10px;
          font-size: 13.5px; color: var(--vapor); cursor: pointer; border-radius: 9px; transition: color 220ms ease;
          font-family: inherit;
        }
        .segmented-btn.active { color: var(--pulse-ink); font-weight: 600; }
        .segmented-btn:active { transform: scale(0.97); }
        .chip-desc { font-size: 13px; color: var(--vapor-dim); margin-top: 10px; min-height: 18px; line-height: 1.5; }

        .cta-button {
          background: var(--pulse); color: var(--pulse-ink); border: none;
          font-weight: 600; font-size: 15px; padding: 14px 28px; border-radius: 12px; cursor: pointer;
          display: inline-flex; align-items: center; gap: 9px; font-family: inherit;
          box-shadow: inset 0 1px 0 rgba(255,255,255,0.3), 0 1px 2px rgba(0,0,0,0.3);
          transition: transform 130ms cubic-bezier(0.22,1,0.36,1), filter 180ms ease;
        }
        .cta-button:hover { filter: brightness(1.05); }
        .cta-button:active { transform: scale(0.97); }
        .cta-button:disabled { opacity: 0.4; cursor: not-allowed; }

        .ghost-button {
          background: transparent; color: var(--vapor); border: 1px solid var(--line);
          font-size: 13.5px; padding: 10px 16px; border-radius: 10px; cursor: pointer; font-family: inherit;
          display: inline-flex; align-items: center; gap: 7px;
          transition: transform 130ms cubic-bezier(0.22,1,0.36,1), border-color 180ms ease, color 180ms ease;
        }
        .ghost-button:hover { color: var(--paper); border-color: var(--line-bright); }
        .ghost-button:active { transform: scale(0.96); }

        /* loading */
        .loading-stage { max-width: 480px; margin: 60px auto; text-align: center; }
        .loading-mark {
          width: 46px; height: 46px; border-radius: 50%; margin: 0 auto 24px;
          border: 1px solid var(--line-bright); display: flex; align-items: center; justify-content: center;
          position: relative;
        }
        .loading-mark::before {
          content: ""; width: 6px; height: 6px; border-radius: 50%; background: var(--pulse);
          animation: soft-pulse 1.6s ease-in-out infinite;
        }
        @keyframes soft-pulse { 0%,100% { opacity: 0.35; transform: scale(0.9); } 50% { opacity: 1; transform: scale(1.15); } }
        .loading-text { font-family: "IBM Plex Mono", ui-monospace, monospace; color: var(--vapor); font-size: 13.5px; letter-spacing: 0.02em; min-height: 20px; }

        /* case */
        .case-card { max-width: 700px; margin: 14px auto 0; position: relative; }
        .folio {
          position: absolute; top: -10px; right: 4px; font-size: 76px; line-height: 1;
          font-style: italic; font-weight: 400; color: var(--paper); opacity: 0.045;
          pointer-events: none; user-select: none; letter-spacing: -0.02em;
        }
        .case-meta { display: flex; align-items: center; justify-content: space-between; margin-bottom: 20px; position: relative; }
        .case-id { font-family: "IBM Plex Mono", ui-monospace, monospace; font-size: 12.5px; color: var(--vapor); letter-spacing: 0.03em; }
        .timer { font-family: "IBM Plex Mono", ui-monospace, monospace; font-size: 12.5px; color: var(--pulse); }

        .panel-block {
          background: var(--panel); border: 1px solid var(--line); border-top-color: var(--line-bright); border-radius: 16px;
          padding: 26px 28px; margin-bottom: 16px;
          backdrop-filter: blur(20px) saturate(160%);
          box-shadow: 0 24px 60px -28px rgba(0,0,0,0.6), inset 0 1px 0 rgba(255,255,255,0.05);
          position: relative;
        }
        .id-line { font-size: 15px; color: var(--vapor); margin: 0 0 14px; }
        .queixa-wrap { position: relative; padding-left: 18px; }
        .queixa-wrap::before {
          content: "\\201C"; position: absolute; left: -6px; top: -18px;
          font-family: Fraunces, Georgia, serif; font-size: 52px; color: var(--pulse-dim);
        }
        .queixa { font-family: Fraunces, Georgia, serif; font-size: 21px; font-style: italic; font-weight: 500; letter-spacing: -0.01em; line-height: 1.4; margin: 0; color: var(--paper); }

        .reveal-row { display: flex; flex-wrap: wrap; gap: 10px; margin-top: 22px; }
        .section { animation: section-in 380ms cubic-bezier(0.22,1,0.36,1); }
        @keyframes section-in { from { opacity: 0; transform: translateY(-4px); } to { opacity: 1; transform: translateY(0); } }
        .section-title { font-family: "IBM Plex Mono", ui-monospace, monospace; font-size: 11px; text-transform: uppercase; letter-spacing: 0.09em; color: var(--vapor); margin: 0 0 9px; }
        .section-body { font-size: 15.5px; line-height: 1.65; color: var(--paper); margin: 0; }

        .hypothesis-block { margin-top: 22px; }
        textarea.hyp-input {
          width: 100%; min-height: 96px; resize: vertical; background: var(--ink-2); border: 1px solid var(--line); border-radius: 12px;
          color: var(--paper); font-size: 15px; padding: 15px; line-height: 1.55; font-family: inherit;
          transition: border-color 200ms ease, box-shadow 200ms ease;
        }
        textarea.hyp-input:focus { outline: none; border-color: var(--pulse); box-shadow: 0 0 0 3px rgba(70,224,176,0.12); }
        textarea.hyp-input::placeholder { color: var(--vapor-dim); }

        /* discussion */
        .diag-badge {
          display: inline-flex; align-items: center; gap: 8px; background: rgba(70,224,176,0.12); border: 1px solid var(--pulse-dim);
          color: var(--pulse); padding: 9px 18px; border-radius: 999px; font-weight: 600; font-size: 14.5px; margin-bottom: 6px;
        }
        .discussion-title { font-family: "IBM Plex Mono", ui-monospace, monospace; font-size: 11px; text-transform: uppercase; letter-spacing: 0.09em; color: var(--vapor); margin: 0 0 9px; }
        .reasoning-text { font-size: 15.5px; line-height: 1.68; color: var(--paper); margin: 0; }
        .key-points { margin: 10px 0 0; padding-left: 0; list-style: none; }
        .key-points li { display: flex; gap: 11px; font-size: 14.5px; line-height: 1.55; color: var(--paper); margin-bottom: 10px; }
        .key-points svg { flex-shrink: 0; margin-top: 2px; color: var(--pulse); }

        .footer-note { margin-top: 28px; font-size: 12px; color: var(--vapor-dim); line-height: 1.55; max-width: 62ch; }
        .footer-note::before { content: ""; display: block; width: 26px; height: 1px; background: var(--line); margin-bottom: 12px; }

        .error-box { max-width: 520px; margin: 70px auto; text-align: center; color: var(--vapor); }
        .error-box svg { color: var(--critical); margin-bottom: 12px; }

        button:focus-visible, textarea:focus-visible { outline: 2px solid var(--pulse); outline-offset: 2px; }

        @media (max-width: 600px) {
          .chrome, .stage { padding-left: 20px; padding-right: 20px; }
          .panel-block { padding: 22px 18px; }
          .folio { font-size: 54px; }
        }

        @media (prefers-reduced-motion: reduce) {
          .loading-mark::before { animation: none !important; }
          .section, .stage-panel { animation: none !important; }
          .diferencial-app { transition: opacity 200ms ease !important; transform: none !important; }
        }
      `}</style>

      <link rel="preconnect" href="https://fonts.googleapis.com" />
      <link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,400;0,9..144,500;0,9..144,600;1,9..144,500&family=IBM+Plex+Mono:wght@400;500;600&display=swap" rel="stylesheet" />

      <div className="chrome">
        <div className="brand">
          <div className="brand-mark">D</div>
          <div className="brand-text">
            <span className="brand-name display-face">Diferencial</span>
            <span className="brand-sub">Raciocínio clínico</span>
          </div>
        </div>
        <div className="stats-chip">{stats.casesCompleted || 0} casos concluídos</div>
      </div>

      <div className="stage">
        {stage === "setup" && (
          <div className="setup-card stage-panel">
            <p className="eyebrow">Treino de raciocínio clínico</p>
            <h1 className="setup-title display-face">Pratique o raciocínio, não a memorização.</h1>
            <p className="setup-sub">Cada tentativa gera um caso novo. Colete a história, o exame e os exames complementares antes de fechar a hipótese — como no plantão.</p>

            <div className="field-group">
              <span className="field-label">Especialidade</span>
              <div className="chip-row">
                {ESPECIALIDADES.map((esp) => (
                  <button key={esp} className={`chip ${especialidade === esp ? "selected" : ""}`} onClick={() => setEspecialidade(esp)}>
                    {esp}
                  </button>
                ))}
              </div>
            </div>

            <div className="field-group">
              <span className="field-label">Nível</span>
              <div className="segmented">
                <div className="segmented-indicator" style={{ transform: `translateX(${nivelIndex * 100}%)` }} />
                {NIVEIS.map((n) => (
                  <button key={n.id} className={`segmented-btn ${nivel === n.id ? "active" : ""}`} onClick={() => setNivel(n.id)}>
                    {n.label}
                  </button>
                ))}
              </div>
              <p className="chip-desc">{NIVEIS.find((n) => n.id === nivel)?.desc}</p>
            </div>

            <button className="cta-button" onClick={handleGerar}>
              <Stethoscope size={17} /> Gerar caso
            </button>
          </div>
        )}

        {stage === "loading" && (
          <div className="loading-stage stage-panel">
            <div className="loading-mark" />
            <p className="loading-text">{LOADING_MESSAGES[loadingMsgIdx]}</p>
          </div>
        )}

        {stage === "error" && (
          <div className="error-box stage-panel">
            <AlertCircle size={26} />
            <p>Não foi possível gerar o caso agora. Tente novamente.</p>
            <button className="cta-button" style={{ marginTop: 14 }} onClick={handleGerar}>Tentar novamente</button>
          </div>
        )}

        {stage === "case" && caso && (
          <div className="case-card stage-panel">
            <span className="folio display-face">{String(caseNumber).padStart(2, "0")}</span>
            <div className="case-meta">
              <span className="case-id">CASO Nº {String(caseNumber).padStart(3, "0")} · {especialidade.toUpperCase()}</span>
              <span className="timer">{mm}:{ss}</span>
            </div>

            <div className="panel-block">
              <p className="id-line">{caso.identificacao}</p>
              <div className="queixa-wrap">
                <p className="queixa">{caso.queixaPrincipal}</p>
              </div>

              <div className="reveal-row">
                {!revealed.hist && (
                  <button className="ghost-button" onClick={() => setRevealed((r) => ({ ...r, hist: true }))}>
                    <ClipboardList size={14} /> Ver história e antecedentes <ChevronDown size={13} />
                  </button>
                )}
                {!revealed.exame && (
                  <button className="ghost-button" onClick={() => setRevealed((r) => ({ ...r, exame: true }))}>
                    <Stethoscope size={14} /> Ver exame físico <ChevronDown size={13} />
                  </button>
                )}
                {!revealed.exames && (
                  <button className="ghost-button" onClick={() => setRevealed((r) => ({ ...r, exames: true }))}>
                    <FlaskConical size={14} /> Ver exames complementares <ChevronDown size={13} />
                  </button>
                )}
              </div>

              {revealed.hist && (
                <div className="section">
                  <Ruler />
                  <p className="section-title">História e antecedentes</p>
                  <p className="section-body">{caso.hda} {caso.antecedentes}</p>
                </div>
              )}
              {revealed.exame && (
                <div className="section">
                  <Ruler />
                  <p className="section-title">Exame físico</p>
                  <p className="section-body">{caso.exameFisico}</p>
                </div>
              )}
              {revealed.exames && (
                <div className="section">
                  <Ruler />
                  <p className="section-title">Exames complementares</p>
                  <p className="section-body">{caso.examesComplementares}</p>
                </div>
              )}
            </div>

            <div className="hypothesis-block">
              <span className="field-label">Sua hipótese diagnóstica e conduta inicial</span>
              <textarea
                className="hyp-input"
                placeholder="Descreva sua hipótese principal e o próximo passo que você tomaria…"
                value={hipotese}
                onChange={(e) => setHipotese(e.target.value)}
              />
              <button className="cta-button" style={{ marginTop: 14 }} disabled={hipotese.trim().length === 0} onClick={handleConfirmar}>
                <Check size={17} /> Confirmar hipótese
              </button>
            </div>

            <p className="footer-note">Caso gerado por IA, com fins educacionais. Pode conter imprecisões — confronte sempre com fontes e diretrizes oficiais.</p>
          </div>
        )}

        {stage === "discussion" && caso && (
          <div className="case-card stage-panel">
            <span className="folio display-face">{String(caseNumber).padStart(2, "0")}</span>
            <div className="case-meta">
              <span className="case-id">CASO Nº {String(caseNumber).padStart(3, "0")} · {especialidade.toUpperCase()}</span>
              <span className="timer">{mm}:{ss}</span>
            </div>

            <div className="panel-block">
              <div className="diag-badge"><Check size={15} /> {caso.diagnostico}</div>
              <Ruler />
              <p className="discussion-title">Sua hipótese</p>
              <p className="reasoning-text" style={{ color: "var(--vapor)" }}>{hipotese}</p>

              <div style={{ marginTop: 20 }}>
                <p className="discussion-title">Raciocínio clínico</p>
                <p className="reasoning-text">{caso.raciocinio}</p>
              </div>

              <div style={{ marginTop: 20 }}>
                <p className="discussion-title">Pontos-chave</p>
                <ul className="key-points">
                  {(caso.pontosChave || []).map((p, i) => (
                    <li key={i}><Check size={15} />{p}</li>
                  ))}
                </ul>
              </div>
            </div>

            <button className="cta-button" onClick={handleNovoCaso}>
              <RotateCcw size={17} /> Gerar novo caso
            </button>

            <p className="footer-note">Caso gerado por IA, com fins educacionais. Pode conter imprecisões — confronte sempre com fontes e diretrizes oficiais.</p>
          </div>
        )}
      </div>
    </div>
  );
}