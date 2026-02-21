import { useState } from "react";

const SYSTEM_PROMPT = `You are a professional GitHub profile README writer. 
Generate a stunning, creative GitHub profile README.md for a Data Science / ML Junior developer.

Person info:
- Name: شادية (Shadya)
- Gender: Female
- Level: Junior (1-2 years experience)
- Main field: Data Science / Machine Learning
- Tech stack: Python, Pandas, NumPy, Scikit-learn, Matplotlib, Seaborn, Jupyter Notebook

Generate a GitHub profile README.md that includes:
1. A creative header with name "شادية | Shadya" and a catchy tagline about Data Science
2. A short "About Me" section (mix of Arabic and English, friendly and warm tone, female perspective)
3. Tech stack section with markdown badges (use shields.io badge syntax like ![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white))
4. Include badges for: Python, Pandas, NumPy, Scikit-learn, Jupyter, Matplotlib, Seaborn, Git, GitHub, VS Code
5. A "What I'm working on" section mentioning ML projects and data analysis
6. A "Currently learning" section (Deep Learning, SQL, Data Visualization)
7. GitHub stats section using github-readme-stats (use the placeholder username "shadya-ds")
8. A fun fact or inspiring quote about data science at the end
9. Contact/reach me section with placeholder links for LinkedIn and Email

Make it visually rich with emojis, well-structured, modern GitHub profile style.
The README should feel personal, warm, and professional at the same time.
Write it fully in markdown, ready to paste directly into GitHub profile README.`;

export default function PortfolioReadme() {
  const [readme, setReadme] = useState("");
  const [loading, setLoading] = useState(false);
  const [copied, setCopied] = useState(false);

  const generate = async () => {
    setLoading(true);
    setReadme("");
    setCopied(false);

    try {
      const response = await fetch("https://api.anthropic.com/v1/messages", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          model: "claude-sonnet-4-20250514",
          max_tokens: 1000,
          system: SYSTEM_PROMPT,
          messages: [{ role: "user", content: "Generate the GitHub profile README for شادية now. Make it beautiful and complete." }],
        }),
      });

      const data = await response.json();
      const text = data.content?.map((b) => b.text || "").join("") || "";
      setReadme(text);
    } catch (err) {
      setReadme("❌ حصل خطأ. حاولي تاني.");
    }
    setLoading(false);
  };

  const copy = () => {
    navigator.clipboard.writeText(readme);
    setCopied(true);
    setTimeout(() => setCopied(false), 2500);
  };

  return (
    <div style={{
      minHeight: "100vh",
      background: "linear-gradient(135deg, #0f0c29, #302b63, #24243e)",
      fontFamily: "'JetBrains Mono', 'Courier New', monospace",
      color: "#e2e8f0",
      padding: "40px 24px",
    }}>
      <div style={{ maxWidth: 860, margin: "0 auto" }}>

        {/* Header */}
        <div style={{ textAlign: "center", marginBottom: 48 }}>
          <div style={{
            display: "inline-flex",
            alignItems: "center",
            gap: 10,
            background: "rgba(255,255,255,0.05)",
            border: "1px solid rgba(255,255,255,0.1)",
            borderRadius: 50,
            padding: "8px 20px",
            fontSize: 13,
            color: "#a78bfa",
            marginBottom: 20,
          }}>
            ✨ GitHub Profile README Generator
          </div>
          <h1 style={{
            fontSize: 42,
            fontWeight: 800,
            margin: "0 0 12px",
            background: "linear-gradient(90deg, #a78bfa, #f472b6, #60a5fa)",
            WebkitBackgroundClip: "text",
            WebkitTextFillColor: "transparent",
          }}>
            مرحباً شادية 👩‍💻
          </h1>
          <p style={{ color: "#94a3b8", fontSize: 15, margin: 0 }}>
            اضغطي Generate وهيتكتبلك README بورتفوليو GitHub جاهز للنسخ
          </p>
        </div>

        {/* Profile summary cards */}
        <div style={{
          display: "grid",
          gridTemplateColumns: "repeat(3, 1fr)",
          gap: 16,
          marginBottom: 36,
        }}>
          {[
            { icon: "🎯", label: "المجال", value: "Data Science / ML" },
            { icon: "📈", label: "المستوى", value: "Junior (1-2 سنة)" },
            { icon: "🐍", label: "التقنية", value: "Python & ML libs" },
          ].map((c) => (
            <div key={c.label} style={{
              background: "rgba(255,255,255,0.04)",
              border: "1px solid rgba(167,139,250,0.2)",
              borderRadius: 12,
              padding: "16px 20px",
              textAlign: "center",
            }}>
              <div style={{ fontSize: 24, marginBottom: 8 }}>{c.icon}</div>
              <div style={{ fontSize: 11, color: "#94a3b8", marginBottom: 4 }}>{c.label}</div>
              <div style={{ fontSize: 13, fontWeight: 600, color: "#e2e8f0" }}>{c.value}</div>
            </div>
          ))}
        </div>

        {/* Generate Button */}
        <div style={{ textAlign: "center", marginBottom: 36 }}>
          <button
            onClick={generate}
            disabled={loading}
            style={{
              background: loading
                ? "rgba(255,255,255,0.05)"
                : "linear-gradient(135deg, #7c3aed, #a855f7)",
              color: loading ? "#64748b" : "#fff",
              border: "none",
              borderRadius: 12,
              padding: "14px 40px",
              fontSize: 15,
              fontWeight: 700,
              cursor: loading ? "not-allowed" : "pointer",
              fontFamily: "inherit",
              boxShadow: loading ? "none" : "0 0 30px rgba(167,139,250,0.3)",
              transition: "all 0.3s",
              display: "inline-flex",
              alignItems: "center",
              gap: 10,
            }}
          >
            {loading ? (
              <>
                <span style={{
                  display: "inline-block",
                  width: 16,
                  height: 16,
                  border: "2px solid #a78bfa",
                  borderTopColor: "transparent",
                  borderRadius: "50%",
                  animation: "spin 0.8s linear infinite",
                }} />
                بيتكتب README...
              </>
            ) : "✨ Generate README"}
          </button>
        </div>

        <style>{`@keyframes spin { to { transform: rotate(360deg); } }`}</style>

        {/* Output */}
        {readme && (
          <div style={{
            background: "rgba(15, 15, 30, 0.8)",
            border: "1px solid rgba(167,139,250,0.3)",
            borderRadius: 14,
            overflow: "hidden",
            boxShadow: "0 0 40px rgba(167,139,250,0.1)",
          }}>
            {/* Toolbar */}
            <div style={{
              display: "flex",
              justifyContent: "space-between",
              alignItems: "center",
              padding: "12px 20px",
              borderBottom: "1px solid rgba(255,255,255,0.07)",
              background: "rgba(255,255,255,0.03)",
            }}>
              <div style={{ display: "flex", alignItems: "center", gap: 8 }}>
                <span style={{ fontSize: 13, color: "#a78bfa" }}>📄</span>
                <span style={{ fontSize: 12, color: "#94a3b8" }}>README.md — جاهز للنسخ</span>
              </div>
              <button
                onClick={copy}
                style={{
                  background: copied
                    ? "linear-gradient(135deg, #16a34a, #22c55e)"
                    : "rgba(255,255,255,0.06)",
                  border: "1px solid rgba(255,255,255,0.1)",
                  color: copied ? "#fff" : "#94a3b8",
                  borderRadius: 8,
                  padding: "6px 18px",
                  fontSize: 12,
                  cursor: "pointer",
                  fontFamily: "inherit",
                  fontWeight: 600,
                  transition: "all 0.2s",
                }}
              >
                {copied ? "✓ تم النسخ!" : "📋 Copy"}
              </button>
            </div>

            {/* Content */}
            <pre style={{
              padding: "24px",
              margin: 0,
              whiteSpace: "pre-wrap",
              wordBreak: "break-word",
              fontSize: 13,
              lineHeight: 1.8,
              color: "#cbd5e1",
              maxHeight: 560,
              overflowY: "auto",
            }}>
              {readme}
            </pre>
          </div>
        )}

        {/* Footer note */}
        {readme && (
          <p style={{ textAlign: "center", color: "#64748b", fontSize: 12, marginTop: 20 }}>
            💡 بعد النسخ، روحي على GitHub → Settings → Repositories → اعملي repo باسمك → حطي README.md فيه
          </p>
        )}
      </div>
    </div>
  );
}
