import React, { useState } from "react";
import { motion } from "framer-motion";

// Single-file React component for a clean, engaging math education site.
// Uses Tailwind CSS utility classes for styling. Default export is the page component.

export default function MathEduSite() {
  const [route, setRoute] = useState("home");
  const [selectedTopic, setSelectedTopic] = useState(null);
  const [quizState, setQuizState] = useState({ q: 0, score: 0, finished: false });

  const topics = [
    {
      id: "numbers",
      title: "Sense Angka",
      blurb:
        "Membangun intuisi tentang bilangan, perkalian, pembagian, dan pola — dasar dari semua matematika.",
      lessons: [
        {
          title: "Trik Perkalian Cepat",
          content:
            "Gunakan pemecahan menjadi (a+b)(c+d) atau gunakan pengelompokan. Contoh: 27×13 = 27×(10+3) = 270 + 81 = 351.",
        },
        {
          title: "Pola Bilangan",
          content:
            "Cari pola aritmetika/geometri untuk memprediksi suku berikutnya. Berlatih dengan deret Fibonacci dan segitiga Pascal.",
        },
      ],
    },
    {
      id: "geometry",
      title: "Geometri Visual",
      blurb: "Bangun rasa ruang: luas, keliling, sudut, dan bukti visual yang menawan.",
      lessons: [
        {
          title: "Area & Bukti Visual",
          content:
            "Menggunakan potongan dan penggabungan untuk melihat mengapa rumus area bekerja — contohnya, mengubah segitiga menjadi persegi panjang.",
        },
      ],
    },
    {
      id: "algebra",
      title: "Aljabar Kreatif",
      blurb: "Simbol sebagai alat: menyusun, menyederhanakan, dan memecahkan masalah dunia nyata.",
      lessons: [
        {
          title: "Persamaan Linear",
          content:
            "Langkah demi langkah menyelesaikan persamaan linear dan memodelkan situasi (kecepatan, biaya, konversi).",
        },
      ],
    },
  ];

  const quiz = [
    {
      q: "Jika 3x + 5 = 20, maka x = ?",
      choices: ["3", "5", "15", "-5"],
      a: 1,
    },
    {
      q: "Luas segitiga dengan alas 8 dan tinggi 5 adalah...",
      choices: ["13", "20", "40", "10"],
      a: 3,
    },
  ];

  function startQuiz() {
    setQuizState({ q: 0, score: 0, finished: false });
    setRoute("quiz");
  }

  function answerQuiz(i) {
    const correct = quiz[quizState.q].a === i;
    setQuizState((s) => ({ ...s, score: s.score + (correct ? 1 : 0) }));
    if (quizState.q + 1 >= quiz.length) {
      setQuizState((s) => ({ ...s, finished: true }));
    } else {
      setQuizState((s) => ({ ...s, q: s.q + 1 }));
    }
  }

  function QuadraticSolver() {
    const [a, setA] = useState("1");
    const [b, setB] = useState("-3");
    const [c, setC] = useState("2");
    const [res, setRes] = useState(null);

    function solve() {
      const A = parseFloat(a);
      const B = parseFloat(b);
      const C = parseFloat(c);
      const disc = B * B - 4 * A * C;
      if (isNaN(A) || isNaN(B) || isNaN(C)) return setRes({ err: "Masukkan angka valid" });
      if (disc < 0) return setRes({ msg: "Akar imajiner (diskriminan < 0)" });
      const x1 = ((-B + Math.sqrt(disc)) / (2 * A)).toFixed(5);
      const x2 = ((-B - Math.sqrt(disc)) / (2 * A)).toFixed(5);
      setRes({ x1, x2, disc: disc.toFixed(5) });
    }

    return (
      <div className="p-4 rounded-xl border">
        <h3 className="text-lg font-semibold mb-2">Pemecah Persamaan Kuadrat</h3>
        <p className="text-sm mb-2">Masukkan koefisien (a, b, c) lalu tekan <em>Solve</em>.</p>
        <div className="grid grid-cols-3 gap-2 mb-3">
          <input className="p-2 border rounded" value={a} onChange={(e) => setA(e.target.value)} />
          <input className="p-2 border rounded" value={b} onChange={(e) => setB(e.target.value)} />
          <input className="p-2 border rounded" value={c} onChange={(e) => setC(e.target.value)} />
        </div>
        <button onClick={solve} className="px-4 py-2 rounded shadow bg-gradient-to-r from-indigo-500 to-purple-500 text-white">Solve</button>
        {res && (
          <div className="mt-3 p-3 bg-slate-50 rounded">
            {res.err && <div className="text-red-600">{res.err}</div>}
            {res.msg && <div className="text-orange-600">{res.msg}</div>}
            {res.x1 && (
              <div>
                <div>x₁ = {res.x1}</div>
                <div>x₂ = {res.x2}</div>
                <div className="text-sm text-gray-500">Diskriminan = {res.disc}</div>
              </div>
            )}
          </div>
        )}
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-gradient-to-b from-white to-slate-50 text-slate-800 p-6">
      <header className="max-w-4xl mx-auto flex items-center justify-between mb-6">
        <div>
          <h1 className="text-2xl font-extrabold">Matematika Ceria</h1>
          <p className="text-sm text-gray-600">Belajar matematika dengan cara yang visual, singkat, dan menantang.</p>
        </div>
        <nav className="space-x-3">
          <button onClick={() => { setRoute('home'); setSelectedTopic(null); }} className="px-3 py-1 rounded hover:bg-slate-100">Home</button>
          <button onClick={() => { setRoute('topics'); }} className="px-3 py-1 rounded hover:bg-slate-100">Topik</button>
          <button onClick={() => startQuiz()} className="px-3 py-1 rounded bg-indigo-600 text-white">Quiz</button>
        </nav>
      </header>

      <main className="max-w-4xl mx-auto">
        {route === 'home' && (
          <section>
            <motion.div initial={{ y: -10, opacity: 0 }} animate={{ y: 0, opacity: 1 }} className="bg-white p-6 rounded-2xl shadow-md mb-6">
              <div className="grid md:grid-cols-2 gap-6 items-center">
                <div>
                  <h2 className="text-3xl font-bold mb-2">Matematika bukan sekadar angka — tetapi cara berpikir.</h2>
                  <p className="text-gray-600 mb-4">Mulai dari rasa ingin tahu kecil sampai tantangan logika besar — semuanya akan kita pecahkan pelan-pelan, dengan contoh nyata dan latihan pendek setiap hari.</p>
                  <div className="flex gap-3">
                    <button onClick={() => setRoute('topics')} className="px-4 py-2 rounded shadow bg-gradient-to-r from-emerald-400 to-cyan-500 text-white">Jelajah Topik</button>
                    <button onClick={() => setRoute('tools')} className="px-4 py-2 rounded border">Alat Interaktif</button>
                  </div>
                </div>
                <div className="p-4 bg-gradient-to-br from-purple-50 to-indigo-50 rounded-lg">
                  <div className="text-center py-8">
                    <div className="text-6xl font-extrabold">📐</div>
                    <p className="mt-3 text-sm text-gray-600">Masukkan konsep, dapatkan pemahaman.</p>
                  </div>
                </div>
              </div>
            </motion.div>

            <section className="grid md:grid-cols-3 gap-4">
              {topics.map((t) => (
                <motion.article key={t.id} whileHover={{ y: -6 }} className="bg-white p-4 rounded-xl shadow cursor-pointer" onClick={() => { setSelectedTopic(t); setRoute('topic'); }}>
                  <h3 className="font-semibold text-lg">{t.title}</h3>
                  <p className="text-sm text-gray-600 mt-2">{t.blurb}</p>
                  <div className="mt-3 text-xs text-gray-400">{t.lessons.length} pelajaran • Tantangan harian</div>
                </motion.article>
              ))}
            </section>

            <section className="mt-6 p-4 bg-white rounded-xl shadow">
              <h3 className="font-semibold text-lg mb-2">Masalah Hari Ini</h3>
              <p>Jika kamu berjalan 3 km/jam, berapa lama untuk menempuh jarak 7,5 km? Coba pikirkan tanpa kalkulator.</p>
              <div className="mt-3 flex gap-2">
                <button onClick={() => alert('Jawaban: 2,5 jam — karena 7.5 / 3 = 2.5')} className="px-3 py-1 rounded border">Tunjukkan Jawaban</button>
                <button onClick={() => setRoute('puzzle')} className="px-3 py-1 rounded bg-indigo-600 text-white">Buka Puzzle</button>
              </div>
            </section>
          </section>
        )}

        {route === 'topics' && (
          <section>
            <h2 className="text-xl font-semibold mb-3">Daftar Topik</h2>
            <div className="grid md:grid-cols-3 gap-4">
              {topics.map((t) => (
                <div key={t.id} className="bg-white p-4 rounded-xl shadow">
                  <h4 className="font-semibold">{t.title}</h4>
                  <p className="text-sm text-gray-600 mt-1">{t.blurb}</p>
                  <div className="mt-3 flex justify-between items-center">
                    <span className="text-xs text-gray-400">{t.lessons.length} pelajaran</span>
                    <button onClick={() => { setSelectedTopic(t); setRoute('topic'); }} className="px-3 py-1 rounded border text-sm">Buka</button>
                  </div>
                </div>
              ))}
            </div>
          </section>
        )}

        {route === 'topic' && selectedTopic && (
          <section>
            <button onClick={() => setRoute('topics')} className="text-sm text-indigo-600 mb-3">← Kembali ke Topik</button>
            <div className="bg-white p-6 rounded-xl shadow">
              <h2 className="text-2xl font-bold">{selectedTopic.title}</h2>
              <p className="text-gray-600 mt-1">{selectedTopic.blurb}</p>
              <div className="mt-4">
                {selectedTopic.lessons.map((l, i) => (
                  <article key={i} className="mb-4">
                    <h3 className="font-semibold">{l.title}</h3>
                    <p className="text-sm text-gray-700 mt-1">{l.content}</p>
                  </article>
                ))}
              </div>
            </div>
          </section>
        )}

        {route === 'tools' && (
          <section>
            <h2 className="text-xl font-semibold mb-3">Alat Interaktif</h2>
            <div className="grid md:grid-cols-2 gap-4">
              <div className="bg-white p-4 rounded-xl shadow">
                <QuadraticSolver />
              </div>

              <div className="bg-white p-4 rounded-xl shadow">
                <h3 className="text-lg font-semibold mb-2">Kalkulator Sederhana</h3>
                <p className="text-sm text-gray-600">Coba ketik ekspresi matematika sederhana — contohnya: 12*3+5</p>
                <ExpressionCalculator />
              </div>
            </div>
          </section>
        )}

        {route === 'puzzle' && (
          <section>
            <h2 className="text-xl font-semibold">Puzzle Visual</h2>
            <div className="bg-white p-4 rounded-xl shadow mt-3">
              <p>Potong persegi panjang menjadi dua bagian, susun ulang menjadi segitiga — lihat bagaimana area tetap sama. (Bayangkan visualnya!)</p>
              <p className="mt-2 text-sm text-gray-500">Tips: Gambar di kertas akan sangat membantu — coba potong dan pindahkan.</p>
            </div>
          </section>
        )}

        {route === 'quiz' && (
          <section>
            <h2 className="text-xl font-semibold mb-3">Quiz Cepat</h2>
            <div className="bg-white p-4 rounded-xl shadow">
              {!quizState.finished ? (
                <div>
                  <div className="mb-2 text-sm text-gray-500">Pertanyaan {quizState.q + 1} dari {quiz.length}</div>
                  <div className="font-semibold">{quiz[quizState.q].q}</div>
                  <div className="mt-3 grid gap-2">
                    {quiz[quizState.q].choices.map((c, i) => (
                      <button key={i} onClick={() => answerQuiz(i)} className="text-left p-2 rounded border">{c}</button>
                    ))}
                  </div>
                </div>
              ) : (
                <div>
                  <div className="text-lg font-semibold">Selesai!</div>
                  <div className="mt-2">Skor kamu: {quizState.score} / {quiz.length}</div>
                  <div className="mt-3 flex gap-2">
                    <button onClick={() => startQuiz()} className="px-3 py-1 rounded bg-indigo-600 text-white">Ulangi</button>
                    <button onClick={() => setRoute('home')} className="px-3 py-1 rounded border">Kembali</button>
                  </div>
                </div>
              )}
            </div>
          </section>
        )}

        <footer className="mt-8 text-center text-sm text-gray-500">
          Dibuat dengan ♡ — Jadikan matematika menyenangkan. | Tips: baca 10-15 menit tiap hari.
        </footer>
      </main>
    </div>
  );
}


function ExpressionCalculator() {
  const [expr, setExpr] = useState("");
  const [out, setOut] = useState(null);

  function evalExpr() {
    try {
      // safe-ish evaluator: allow digits and basic operators only
      if (!/^[0-9+\-*/(). \t]+$/.test(expr)) { setOut({ err: 'Ekspresi mengandung karakter tidak diizinkan' }); return; }
      // eslint-disable-next-line no-eval
      const val = eval(expr);
      setOut({ val });
    } catch (e) {
      setOut({ err: 'Ekspresi tidak valid' });
    }
  }

  return (
    <div>
      <input value={expr} onChange={(e) => setExpr(e.target.value)} className="w-full p-2 border rounded mb-2" placeholder="Contoh: 12*3+5" />
      <div className="flex gap-2">
        <button onClick={evalExpr} className="px-3 py-1 rounded bg-emerald-500 text-white">Hitung</button>
        <button onClick={() => { setExpr(''); setOut(null); }} className="px-3 py-1 rounded border">Reset</button>
      </div>
      {out && (
        <div className="mt-3 p-2 rounded bg-slate-50">
          {out.err ? <div className="text-red-600">{out.err}</div> : <div>Hasil: <strong>{String(out.val)}</strong></div>}
        </div>
      )}
    </div>
  );
}
