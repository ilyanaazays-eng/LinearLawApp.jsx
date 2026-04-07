import React, { useState, useMemo } from 'react';
import { LineChart, Calculator, FunctionSquare, ArrowRight, Activity, CheckCircle2, ClipboardList, CheckCircle, XCircle } from 'lucide-react';

const LinearLawApp = () => {
  const [activeTab, setActiveTab] = useState('interactive');

  return (
    <div className="min-h-screen bg-slate-50 text-slate-800 font-sans">
      {/* Header */}
      <header className="bg-indigo-600 text-white shadow-md">
        <div className="max-w-6xl mx-auto px-4 py-6">
          <h1 className="text-3xl font-bold tracking-tight">Menguasai Hukum Linear</h1>
          <p className="text-indigo-100 mt-2">Bab 6: Hukum Linear • Matematik Tambahan Tingkatan 4</p>
        </div>
      </header>

      {/* Navigation */}
      <div className="max-w-6xl mx-auto px-4 py-6">
        <div className="flex flex-wrap gap-4 mb-8">
          <button
            onClick={() => setActiveTab('interactive')}
            className={`flex items-center gap-2 px-6 py-3 rounded-full font-medium transition-all ${
              activeTab === 'interactive'
                ? 'bg-indigo-600 text-white shadow-lg shadow-indigo-200'
                : 'bg-white text-slate-600 hover:bg-indigo-50 border border-slate-200'
            }`}
          >
            <Activity size={20} />
            6.1 Garis Penyuaian Terbaik
          </button>
          <button
            onClick={() => setActiveTab('conversion')}
            className={`flex items-center gap-2 px-6 py-3 rounded-full font-medium transition-all ${
              activeTab === 'conversion'
                ? 'bg-emerald-600 text-white shadow-lg shadow-emerald-200'
                : 'bg-white text-slate-600 hover:bg-emerald-50 border border-slate-200'
            }`}
          >
            <FunctionSquare size={20} />
            6.2 Penukaran Persamaan
          </button>
          <button
            onClick={() => setActiveTab('application')}
            className={`flex items-center gap-2 px-6 py-3 rounded-full font-medium transition-all ${
              activeTab === 'application'
                ? 'bg-blue-600 text-white shadow-lg shadow-blue-200'
                : 'bg-white text-slate-600 hover:bg-blue-50 border border-slate-200'
            }`}
          >
            <Calculator size={20} />
            6.3 Aplikasi Sebenar
          </button>
          <button
            onClick={() => setActiveTab('quiz')}
            className={`flex items-center gap-2 px-6 py-3 rounded-full font-medium transition-all ${
              activeTab === 'quiz'
                ? 'bg-purple-600 text-white shadow-lg shadow-purple-200'
                : 'bg-white text-slate-600 hover:bg-purple-50 border border-slate-200'
            }`}
          >
            <ClipboardList size={20} />
            Latihan & Ujian
          </button>
        </div>

        {/* Content Area */}
        <div className="bg-white rounded-2xl shadow-xl border border-slate-100 overflow-hidden">
          {activeTab === 'interactive' && <InteractiveGraphTab />}
          {activeTab === 'conversion' && <EquationConversionTab />}
          {activeTab === 'application' && <ApplicationTab />}
          {activeTab === 'quiz' && <QuizTab />}
        </div>
      </div>
    </div>
  );
};

// --- TAB 1: 6.1 Line of Best Fit (Interactive Graph) ---
const InteractiveGraphTab = () => {
  // Experimental data (with some noise)
  const data = [
    { x: 1.1, y: 5.1 },
    { x: 2.5, y: 8.0 },
    { x: 4.2, y: 12.5 },
    { x: 6.3, y: 15.9 },
    { x: 9.1, y: 21.0 },
    { x: 12.5, y: 26.6 },
  ];

  const [m, setM] = useState(1.5);
  const [c, setC] = useState(5);

  // Calculate Error (Sum of Squared Residuals)
  const errorScore = useMemo(() => {
    let sum = 0;
    data.forEach((point) => {
      const predictedY = m * point.x + c;
      sum += Math.pow(predictedY - point.y, 2);
    });
    return sum.toFixed(2);
  }, [m, c]);

  // SVG Coordinates setup
  const width = 600;
  const height = 400;
  const padding = 50;
  const maxX = 15;
  const maxY = 35;

  const getSvgX = (x) => padding + (x / maxX) * (width - 2 * padding);
  const getSvgY = (y) => height - padding - (y / maxY) * (height - 2 * padding);

  return (
    <div className="flex flex-col md:flex-row">
      <div className="flex-1 p-8 border-r border-slate-100">
        <h2 className="text-2xl font-bold text-slate-800 mb-2">Garis Penyuaian Terbaik Interaktif</h2>
        <p className="text-slate-500 mb-6">Mencari Garis Lurus Penyuaian Terbaik</p>

        <p className="text-slate-700 mb-6">
          Laras kecerunan (<b>m</b>) dan pintasan-y (<b>c</b>) untuk mencari garis yang paling padan dengan data eksperimen. Cuba dapatkan <b>Skor Ralat</b> sehampir 0 yang mungkin!
        </p>

        <div className="space-y-6">
          <div className="bg-slate-50 p-6 rounded-xl border border-slate-200">
            <label className="block text-sm font-semibold text-slate-700 mb-2">
              Kecerunan (m) = {m.toFixed(2)}
            </label>
            <input
              type="range"
              min="0.5"
              max="3"
              step="0.05"
              value={m}
              onChange={(e) => setM(parseFloat(e.target.value))}
              className="w-full h-2 bg-indigo-200 rounded-lg appearance-none cursor-pointer accent-indigo-600"
            />
          </div>

          <div className="bg-slate-50 p-6 rounded-xl border border-slate-200">
            <label className="block text-sm font-semibold text-slate-700 mb-2">
              Pintasan-y (c) = {c.toFixed(2)}
            </label>
            <input
              type="range"
              min="0"
              max="10"
              step="0.1"
              value={c}
              onChange={(e) => setC(parseFloat(e.target.value))}
              className="w-full h-2 bg-indigo-200 rounded-lg appearance-none cursor-pointer accent-indigo-600"
            />
          </div>

          <div className={`p-4 rounded-xl border flex items-center justify-between ${
             errorScore < 10 ? 'bg-emerald-50 border-emerald-200' : 'bg-red-50 border-red-200'
          }`}>
            <div>
              <p className="text-sm text-slate-500 font-medium">Skor Ralat</p>
              <p className={`text-2xl font-bold ${errorScore < 10 ? 'text-emerald-700' : 'text-red-700'}`}>
                {errorScore}
              </p>
            </div>
            {errorScore < 10 && <CheckCircle2 className="text-emerald-500" size={32} />}
          </div>
        </div>
      </div>

      <div className="flex-1 p-8 bg-slate-50 flex flex-col items-center justify-center">
        <div className="bg-white p-4 rounded-xl shadow-sm border border-slate-200">
          <svg width={width} height={height} className="max-w-full h-auto">
            {/* Grid & Axes */}
            <line x1={padding} y1={height - padding} x2={width - padding} y2={height - padding} stroke="#cbd5e1" strokeWidth="2" />
            <line x1={padding} y1={padding} x2={padding} y2={height - padding} stroke="#cbd5e1" strokeWidth="2" />
            <text x={width - padding + 10} y={height - padding + 5} className="text-xs fill-slate-500">x</text>
            <text x={padding - 15} y={padding - 10} className="text-xs fill-slate-500">y</text>

            {/* Line of best fit */}
            <line 
              x1={getSvgX(0)} 
              y1={getSvgY(c)} 
              x2={getSvgX(maxX)} 
              y2={getSvgY(m * maxX + c)} 
              stroke="#4f46e5" 
              strokeWidth="3" 
              strokeDasharray="5,5"
            />

            {/* Data Points */}
            {data.map((p, i) => (
              <circle key={i} cx={getSvgX(p.x)} cy={getSvgY(p.y)} r="6" fill="#ef4444" className="transition-all duration-300 hover:r-8" />
            ))}
          </svg>
          <div className="text-center mt-4">
            <code className="text-lg font-mono bg-indigo-50 text-indigo-700 px-4 py-2 rounded-lg">
              Y = {m.toFixed(2)}X + {c.toFixed(2)}
            </code>
          </div>
        </div>
      </div>
    </div>
  );
};

// --- TAB 2: 6.2 Equation Conversion ---
const EquationConversionTab = () => {
  const conversions = [
    {
      original: "y = ax^n",
      steps: [
        "Wujudkan log asas 10 pada kedua-dua belah",
        "log(y) = log(ax^n)",
        "log(y) = log(a) + n(log x)"
      ],
      Y: "log(y)",
      X: "log(x)",
      m: "n",
      c: "log(a)"
    },
    {
      original: "y = px + q/x",
      steps: [
        "Darabkan kedua-dua belah dengan x",
        "xy = px² + q"
      ],
      Y: "xy",
      X: "x²",
      m: "p",
      c: "q"
    },
    {
      original: "y = 2x³ - 5x²",
      steps: [
        "Bahagikan kedua-dua belah dengan x²",
        "y/x² = 2x - 5"
      ],
      Y: "y/x²",
      X: "x",
      m: "2",
      c: "-5"
    }
  ];

  return (
    <div className="p-8 bg-emerald-50/30">
      <div className="mb-8">
        <h2 className="text-2xl font-bold text-slate-800">Penukaran kepada Y = mX + c</h2>
        <p className="text-slate-600">Menukarkan persamaan tak linear kepada persamaan linear.</p>
      </div>

      <div className="grid md:grid-cols-3 gap-6">
        {conversions.map((conv, idx) => (
          <div key={idx} className="bg-white rounded-2xl shadow-sm border border-emerald-100 p-6 flex flex-col hover:shadow-md transition-shadow">
            <div className="bg-slate-800 text-white font-mono text-center py-4 rounded-xl text-lg mb-6 shadow-inner">
              {conv.original}
            </div>
            
            <div className="flex-1 space-y-3 mb-6">
              {conv.steps.map((step, i) => (
                <div key={i} className="flex gap-3 text-sm text-slate-600 items-start">
                  <ArrowRight size={16} className="text-emerald-500 mt-0.5 shrink-0" />
                  <span>{step}</span>
                </div>
              ))}
            </div>

            <div className="bg-emerald-50 rounded-xl p-4 border border-emerald-100">
              <div className="grid grid-cols-2 gap-y-2 text-sm">
                <div className="font-semibold text-slate-700">Paksi-Y:</div>
                <div className="font-mono text-emerald-700">{conv.Y}</div>
                
                <div className="font-semibold text-slate-700">Paksi-X:</div>
                <div className="font-mono text-emerald-700">{conv.X}</div>
                
                <div className="font-semibold text-slate-700">Kecerunan (m):</div>
                <div className="font-mono text-emerald-700">{conv.m}</div>
                
                <div className="font-semibold text-slate-700">Pintasan-y (c):</div>
                <div className="font-mono text-emerald-700">{conv.c}</div>
              </div>
            </div>
          </div>
        ))}
      </div>
    </div>
  );
};

// --- TAB 3: 6.3 Real Applications ---
const ApplicationTab = () => {
  return (
    <div className="p-8">
      <div className="mb-8 border-b border-slate-100 pb-8">
        <h2 className="text-2xl font-bold text-slate-800 mb-2">Masalah Fizik Dunia Sebenar</h2>
        <p className="text-slate-600">Mengenal pasti data anomali (Nilai tak tepat).</p>
      </div>

      <div className="grid md:grid-cols-2 gap-8">
        <div>
          <h3 className="text-lg font-bold text-blue-800 mb-4 flex items-center gap-2">
            <Activity size={20} /> Eksperimen
          </h3>
          <p className="text-slate-700 mb-4 leading-relaxed">
            Halaju, <code className="bg-blue-50 px-1 rounded text-blue-700">v</code> bagi satu zarah pada masa <code className="bg-blue-50 px-1 rounded text-blue-700">t</code> diberi oleh persamaan <code className="bg-slate-100 px-2 py-1 rounded font-bold">v = h + kt²</code>. 
            Salah satu halaju yang direkodkan adalah tidak tepat! Bolehkah anda mengesannya menggunakan Hukum Linear?
          </p>

          <div className="overflow-hidden rounded-xl border border-slate-200 shadow-sm mb-6">
            <table className="w-full text-center">
              <thead className="bg-blue-600 text-white">
                <tr>
                  <th className="py-3 px-4">t (s)</th>
                  <th className="py-3 px-4">1</th>
                  <th className="py-3 px-4">2</th>
                  <th className="py-3 px-4">3</th>
                  <th className="py-3 px-4 text-yellow-300">4</th>
                  <th className="py-3 px-4">5</th>
                </tr>
              </thead>
              <tbody className="bg-white divide-y divide-slate-100">
                <tr>
                  <td className="py-3 px-4 font-bold bg-slate-50">v (ms⁻¹)</td>
                  <td className="py-3 px-4">5.6</td>
                  <td className="py-3 px-4">7.1</td>
                  <td className="py-3 px-4">9.2</td>
                  <td className="py-3 px-4 font-bold text-red-500 bg-red-50">13.8 (Ralat!)</td>
                  <td className="py-3 px-4">16.5</td>
                </tr>
                <tr className="bg-blue-50">
                  <td className="py-3 px-4 font-bold">t² (X)</td>
                  <td className="py-3 px-4">1</td>
                  <td className="py-3 px-4">4</td>
                  <td className="py-3 px-4">9</td>
                  <td className="py-3 px-4">16</td>
                  <td className="py-3 px-4">25</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>

        <div className="bg-slate-800 text-white rounded-2xl p-6 shadow-lg">
          <h3 className="text-lg font-bold text-blue-300 mb-4">Langkah Penyelesaian</h3>
          <ol className="space-y-4">
            <li className="flex gap-4">
              <div className="bg-blue-500 w-8 h-8 rounded-full flex items-center justify-center font-bold shrink-0">1</div>
              <div>
                <p className="font-semibold">Tukar kepada Bentuk Linear</p>
                <code className="block mt-1 text-slate-300">v = k(t²) + h</code>
                <p className="text-sm mt-1 text-slate-400">Y = v, X = t², m = k, c = h</p>
              </div>
            </li>
            <li className="flex gap-4">
              <div className="bg-blue-500 w-8 h-8 rounded-full flex items-center justify-center font-bold shrink-0">2</div>
              <div>
                <p className="font-semibold">Kenal Pasti Anomali</p>
                <p className="text-sm mt-1 text-slate-300">Memplot <b>v</b> melawan <b>t²</b> menunjukkan bahawa titik pada t=4 (X=16) berada jauh dari garis lurus.</p>
              </div>
            </li>
            <li className="flex gap-4">
              <div className="bg-blue-500 w-8 h-8 rounded-full flex items-center justify-center font-bold shrink-0">3</div>
              <div>
                <p className="font-semibold">Cari Nilai Sebenar</p>
                <p className="text-sm mt-1 text-slate-300">Daripada Garis Lurus Penyuaian Terbaik yang dibetulkan pada X=16, nilai v yang betul ialah lebih kurang <b>12.4</b> (Bukan 13.8).</p>
              </div>
            </li>
          </ol>
        </div>
      </div>
    </div>
  );
};

// --- TAB 4: Latihan & Ujian ---
const QuizTab = () => {
  const quizData = [
    {
      topic: '6.1',
      title: 'Topik 6.1: Garis Penyuaian Terbaik',
      questions: [
        {
          id: 'q1',
          text: 'Apakah ciri utama graf bagi hubungan linear?',
          options: ['Bentuk Lengkung', 'Garis Lurus', 'Satu Titik', 'Bentuk U'],
          answer: 'Garis Lurus'
        },
        {
          id: 'q2',
          text: 'Jika persamaan garis lurus penyesuaian terbaik ialah Y = 5X - 2, apakah nilai pintasan-y (c)?',
          options: ['5', '-2', '2', 'X'],
          answer: '-2'
        }
      ]
    },
    {
      topic: '6.2',
      title: 'Topik 6.2: Penukaran Persamaan',
      questions: [
        {
          id: 'q3',
          text: 'Tukarkan persamaan tak linear y = ax² + b kepada bentuk linear Y = mX + c. Apakah pembolehubah untuk Paksi-X?',
          options: ['x', 'x²', 'a', 'y'],
          answer: 'x²'
        },
        {
          id: 'q4',
          text: 'Diberi persamaan linear berbentuk xy = 3x - 5. Apakah nilai kecerunan (m)?',
          options: ['-5', 'x', '3', 'xy'],
          answer: '3'
        }
      ]
    },
    {
      topic: '6.3',
      title: 'Topik 6.3: Aplikasi Sebenar',
      questions: [
        {
          id: 'q5',
          text: 'Satu titik didapati direkodkan secara tidak tepat dan berada jauh daripada garis lurus penyuaian terbaik. Titik ini dipanggil sebagai:',
          options: ['Pintasan Paksi', 'Kecerunan', 'Data Anomali (Ralat)', 'Titik Asalan'],
          answer: 'Data Anomali (Ralat)'
        },
        {
          id: 'q6',
          text: 'Berdasarkan graf linear, nilai pintasan-y didapati bernilai 4. Jika formula asal fizik ialah v = kt + h (telah dilinearkan kepada v = k(t) + h), apakah nilai h?',
          options: ['k', 't', '4', '0'],
          answer: '4'
        }
      ]
    }
  ];

  const [answers, setAnswers] = useState({});
  const [submitted, setSubmitted] = useState(false);

  const handleSelect = (qId, option) => {
    if (submitted) return;
    setAnswers(prev => ({ ...prev, [qId]: option }));
  };

  const calculateScores = () => {
    const scores = { '6.1': 0, '6.2': 0, '6.3': 0, total: 0, max: 0 };
    quizData.forEach(section => {
      section.questions.forEach(q => {
        scores.max += 1;
        if (answers[q.id] === q.answer) {
          scores[section.topic] += 1;
          scores.total += 1;
        }
      });
    });
    return scores;
  };

  const scores = submitted ? calculateScores() : null;

  return (
    <div className="p-8 bg-slate-50">
      <div className="mb-8">
        <h2 className="text-2xl font-bold text-slate-800">Uji Kefahaman Anda</h2>
        <p className="text-slate-600">Jawab kuiz di bawah untuk melihat penguasaan anda bagi topik 6.1, 6.2, dan 6.3.</p>
      </div>

      <div className="grid lg:grid-cols-3 gap-8">
        <div className="lg:col-span-2 space-y-8">
          {quizData.map((section, idx) => (
            <div key={idx} className="bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
              <h3 className="text-lg font-bold text-purple-700 border-b border-slate-100 pb-3 mb-4">{section.title}</h3>
              <div className="space-y-6">
                {section.questions.map((q) => (
                  <div key={q.id}>
                    <p className="font-medium text-slate-800 mb-3">{q.text}</p>
                    <div className="grid grid-cols-1 sm:grid-cols-2 gap-3">
                      {q.options.map(opt => (
                        <button
                          key={opt}
                          onClick={() => handleSelect(q.id, opt)}
                          className={`text-left px-4 py-3 rounded-xl border transition-all ${
                            answers[q.id] === opt && !submitted
                              ? 'border-purple-500 bg-purple-50 text-purple-700'
                              : answers[q.id] === opt && submitted && opt === q.answer
                              ? 'border-emerald-500 bg-emerald-50 text-emerald-700'
                              : answers[q.id] === opt && submitted && opt !== q.answer
                              ? 'border-red-500 bg-red-50 text-red-700'
                              : submitted && opt === q.answer
                              ? 'border-emerald-500 bg-emerald-50 text-emerald-700 outline outline-2 outline-emerald-400'
                              : 'border-slate-200 hover:border-purple-300 hover:bg-slate-50 text-slate-600'
                          }`}
                          disabled={submitted}
                        >
                          <div className="flex justify-between items-center">
                            <span>{opt}</span>
                            {submitted && answers[q.id] === opt && opt === q.answer && <CheckCircle size={18} className="text-emerald-600" />}
                            {submitted && answers[q.id] === opt && opt !== q.answer && <XCircle size={18} className="text-red-600" />}
                          </div>
                        </button>
                      ))}
                    </div>
                  </div>
                ))}
              </div>
            </div>
          ))}
          
          {!submitted ? (
            <button 
              onClick={() => {
                if(Object.keys(answers).length < 6) {
                  alert("Sila jawab semua soalan sebelum menghantar.");
                  return;
                }
                setSubmitted(true);
              }}
              className="w-full bg-purple-600 hover:bg-purple-700 text-white font-bold py-4 rounded-xl shadow-lg transition-colors"
            >
              Hantar Jawapan & Semak Skor
            </button>
          ) : (
            <button 
              onClick={() => { setSubmitted(false); setAnswers({}); }}
              className="w-full bg-slate-200 hover:bg-slate-300 text-slate-700 font-bold py-4 rounded-xl transition-colors"
            >
              Cuba Semula Kuiz
            </button>
          )}
        </div>

        {/* Skor Panel */}
        <div className="lg:col-span-1">
          <div className="bg-slate-800 rounded-2xl p-6 text-white shadow-xl sticky top-6">
            <h3 className="text-xl font-bold mb-6 flex items-center gap-2">
              <ClipboardList /> Laporan Prestasi
            </h3>
            
            {submitted ? (
              <div className="space-y-6">
                <div className="text-center p-6 bg-slate-700 rounded-xl">
                  <p className="text-slate-300 mb-1">Skor Keseluruhan</p>
                  <p className="text-5xl font-black text-purple-400">{scores.total}<span className="text-2xl text-slate-400">/{scores.max}</span></p>
                </div>

                <div className="space-y-4">
                  <p className="font-semibold text-slate-300">Prestasi Mengikut Topik:</p>
                  
                  {/* Topik 6.1 */}
                  <div className="bg-slate-700 p-4 rounded-lg flex justify-between items-center">
                    <span className="text-sm">6.1 Penyuaian Terbaik</span>
                    <span className={`font-bold px-3 py-1 rounded-md text-sm ${scores['6.1'] === 2 ? 'bg-emerald-500/20 text-emerald-400' : 'bg-yellow-500/20 text-yellow-400'}`}>
                      {scores['6.1']} / 2
                    </span>
                  </div>
                  
                  {/* Topik 6.2 */}
                  <div className="bg-slate-700 p-4 rounded-lg flex justify-between items-center">
                    <span className="text-sm">6.2 Penukaran Persamaan</span>
                    <span className={`font-bold px-3 py-1 rounded-md text-sm ${scores['6.2'] === 2 ? 'bg-emerald-500/20 text-emerald-400' : 'bg-yellow-500/20 text-yellow-400'}`}>
                      {scores['6.2']} / 2
                    </span>
                  </div>
                  
                  {/* Topik 6.3 */}
                  <div className="bg-slate-700 p-4 rounded-lg flex justify-between items-center">
                    <span className="text-sm">6.3 Aplikasi Sebenar</span>
                    <span className={`font-bold px-3 py-1 rounded-md text-sm ${scores['6.3'] === 2 ? 'bg-emerald-500/20 text-emerald-400' : 'bg-yellow-500/20 text-yellow-400'}`}>
                      {scores['6.3']} / 2
                    </span>
                  </div>
                </div>
              </div>
            ) : (
              <div className="text-center p-8 border-2 border-dashed border-slate-600 rounded-xl text-slate-400">
                <p>Sila jawab soalan di sebelah dan tekan butang "Hantar Jawapan" untuk melihat laporan prestasi anda.</p>
              </div>
            )}
          </div>
        </div>
      </div>
    </div>
  );
};

export default LinearLawApp;
