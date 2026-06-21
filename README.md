<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Ruai干倒白泥 | 泰娱妈咪</title>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+Thai:wght@300;400;600&family=Noto+Serif+SC:wght@400;700&family=Playfair+Display:ital,wght@0,400;0,700;1,400&display=swap" rel="stylesheet">
    <script src="https://cdn.tailwindcss.com"></script>
    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: { 'ruai-bg': '#f3f4f6', 'ruai-text': '#1f2937', 'ruai-accent': '#f472b6', 'ruai-dark-blue': '#1e3a8a' },
                    fontFamily: { sans: ['"Noto Sans Thai"', 'sans-serif'], serif: ['"Playfair Display"', '"Noto Serif SC"', 'serif'] },
                    animation: { 'float': 'float 3s ease-in-out infinite', 'heartbeat': 'heartbeat 1.5s ease-in-out infinite', 'slide-up': 'slideUp 0.5s ease-out forwards', 'fade-in': 'fadeIn 0.5s ease-out forwards' },
                    keyframes: {
                        float: { '0%, 100%': { transform: 'translateY(0)' }, '50%': { transform: 'translateY(-10px)' } },
                        heartbeat: { '0%, 100%': { transform: 'scale(1)' }, '50%': { transform: 'scale(1.1)' } },
                        slideUp: { '0%': { transform: 'translateY(20px)', opacity: '0' }, '100%': { transform: 'translateY(0)', opacity: '1' } },
                        fadeIn: { '0%': { opacity: '0' }, '100%': { opacity: '1' } }
                    }
                }
            }
        }
    </script>
    <style>
        body { overscroll-behavior-y: none; -webkit-tap-highlight-color: transparent; }
        .btn-bounce:active { transform: scale(0.95); }
        .pig-pattern { background-image: url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='%23f472b6' fill-opacity='0.05' fill-rule='evenodd'%3E%3Cpath d='M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z'/%3E%3C/g%3E%3C/svg%3E"); }
        .glass-panel { background: rgba(255, 255, 255, 0.7); backdrop-filter: blur(10px); border: 1px solid rgba(255, 255, 255, 0.5); }
    </style>
</head>
<body class="bg-ruai-bg text-ruai-text font-sans h-screen w-screen overflow-hidden select-none">
    <div id="root"></div>
    <script type="text/babel">
        const { useState, useEffect, useRef } = React;

        // 安全存储函数（防止浏览器沙盒报错白屏）
        const safeGet = (key, defaultVal) => { try { return localStorage.getItem(key) || defaultVal; } catch(e) { return defaultVal; } };
        const safeSet = (key, val) => { try { localStorage.setItem(key, val); } catch(e) { console.warn('Storage blocked'); } };

        const Icon = ({ path, size = 24, fill = "none" }) => (
            <svg width={size} height={size} viewBox="0 0 24 24" fill={fill} stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">{path}</svg>
        );
        const Icons = {
            PigNose: () => <Icon path={<><ellipse cx="12" cy="12" rx="10" ry="6" /><circle cx="8" cy="12" r="1.5" fill="currentColor" stroke="none"/><circle cx="16" cy="12" r="1.5" fill="currentColor" stroke="none"/></>} />,
            Heart: ({filled}) => <Icon fill={filled ? "currentColor" : "none"} path={<path d="M19 14c1.49-1.46 3-3.21 3-5.5A5.5 5.5 0 0 0 16.5 3c-1.76 0-3 .5 4.5 2-1.5-1.5-2.74-2-4.5-2A5.5 5.5 0 0 0 2 8.5c0 2.3 1.5 4.05 3 5.5l7 7Z" />} />,
            Menu: () => <Icon path={<><line x1="4" x2="20" y1="12" y2="12" /><line x1="4" x2="20" y1="6" y2="6" /><line x1="4" x2="20" y1="18" y2="18" /></>} />,
            X: () => <Icon path={<><path d="M18 6 6 18" /><path d="m6 6 12 12" /></>} />,
            Translate: () => <Icon path={<><path d="m5 8 6 6" /><path d="m4 14 6-6 2-3" /><path d="M2 5h12" /><path d="M7 2h1" /><path d="m22 22-5-10-5 10" /><path d="M14 18h6" /></>} />,
            MessageCircle: () => <Icon path={<path d="M7.9 20A9 9 0 1 0 4 16.1L2 22Z" />} />,
            BookOpen: () => <Icon path={<path d="M2 3h6a4 4 0 0 1 4 4v14a3 3 0 0 0-3-3H2z M22 3h-6a4 4 0 0 0-4 4v14a3 3 0 0 1 3-3h7z" />} />,
            Calendar: () => <Icon path={<><rect width="18" height="18" x="3" y="4" rx="2" ry="2" /><line x1="16" x2="16" y1="2" y2="6" /><line x1="8" x2="8" y1="2" y2="6" /><line x1="3" x2="21" y1="10" y2="10" /></>} />,
            Settings: () => <Icon path={<path d="M12.22 2h-.44a2 2 0 0 1-2 2v.18a2 2 0 0 1-1 1.73l-.43.25a2 2 0 0 1-2 0l-.15-.09a2 2 0 0 0-2.73.73l-.22.38a2 2 0 0 0 .73 2.73l.15.1a2 2 0 0 1 1 1.72v.51a2 2 0 0 1-1 1.74l-.15.09a2 2 0 0 0-.73 2.73l.22.38a2 2 0 0 0 2.73.73l.15-.08a2 2 0 0 1 2 0l.43.25a2 2 0 0 1 1 1.73V20a2 2 0 0 0 2 2h.44a2 2 0 0 0 2-2v-.18a2 2 0 0 1 1-1.73l.43-.25a2 2 0 0 1 2 0l.15.09a2 2 0 0 0 2.73-.73l.22-.39a2 2 0 0 0-.73-2.73l-.15-.09a2 2 0 0 1-1-1.72v-.52a2 2 0 0 1 1-1.74l.15-.09a2 2 0 0 0 .73-2.73l-.22-.38a2 2 0 0 0-2.73-.73l-.15.08a2 2 0 0 1-2 0l-.43-.25a2 2 0 0 1-1-1.73V4a2 2 0 0 0-2-2z" />} />,
            Volume2: () => <Icon path={<><polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5" /><path d="M15.54 8.46a5 5 0 0 1 0 7.07" /><path d="M19.07 4.93a10 10 0 0 1 0 14.14" /></>} />,
            Send: () => <Icon path={<><line x1="22" x2="11" y1="2" y2="13" /><polygon points="22 2 15 22 11 13 2 9 22 2" /></>} />
        };

        const ToastContext = React.createContext();
        const ToastProvider = ({ children }) => {
            const [toasts, setToasts] = useState([]);
            const addToast = (msg, type = 'info') => {
                const id = Date.now();
                setToasts(prev => [...prev, { id, msg, type }]);
                setTimeout(() => setToasts(prev => prev.filter(t => t.id !== id)), 3000);
            };
            return (
                <ToastContext.Provider value={addToast}>
                    {children}
                    <div className="fixed top-4 left-1/2 transform -translate-x-1/2 z-50 flex flex-col gap-2 w-full max-w-xs px-4">
                        {toasts.map(t => (
                            <div key={t.id} className={`p-3 rounded-xl shadow-lg text-sm font-medium text-center animate-slide-up glass-panel ${t.type === 'error' ? 'text-red-500 border-red-200' : 'text-ruai-text border-white'}`}>
                                {t.msg}
                            </div>
                        ))}
                    </div>
                </ToastContext.Provider>
            );
        };

        const App = () => {
            const [view, setView] = useState('landing');
            const [apiKey, setApiKey] = useState(safeGet('gemini_api_key', ''));
            const [menuOpen, setMenuOpen] = useState(false);

            useEffect(() => { if (apiKey) safeSet('gemini_api_key', apiKey); }, [apiKey]);

            if (view === 'landing') {
                return (
                    <div className="w-full h-full bg-gradient-to-b from-blue-200 to-white flex flex-col items-center justify-center text-center p-6 relative">
                        <div className="z-10 animate-fade-in flex flex-col items-center">
                            <div className="w-32 h-32 bg-white rounded-full flex items-center justify-center shadow-xl mb-6 animate-heartbeat text-ruai-accent border-4 border-white"><Icons.PigNose /></div>
                            <h1 className="font-serif text-4xl font-bold text-ruai-dark-blue mb-2">Ruai</h1>
                            <p className="font-sans text-gray-500 mb-10 tracking-widest text-sm">THAI FANDOM</p>
                            <button onClick={() => setView('translator')} className="bg-ruai-dark-blue text-white px-10 py-4 rounded-full font-bold shadow-lg btn-bounce flex items-center gap-2">
                                <Icons.Heart filled /> 进入 (Kao Pai)
                            </button>
                        </div>
                    </div>
                );
            }

            return (
                <ToastProvider>
                    <div className="relative w-full h-full bg-ruai-bg pig-pattern flex flex-col">
                        <header className="h-16 flex items-center justify-between px-4 z-20 bg-white/60 backdrop-blur-md shadow-sm">
                            <button onClick={() => setMenuOpen(true)} className="p-2 btn-bounce rounded-full"><Icons.Menu /></button>
                            <h1 className="font-serif font-bold text-lg text-ruai-dark-blue">泰娱妈咪</h1>
                            <button onClick={() => setView('landing')} className="p-2 btn-bounce rounded-full text-ruai-accent"><Icons.PigNose /></button>
                        </header>
                        <main className="flex-1 overflow-y-auto p-4">
                            {view === 'translator' && <Translator apiKey={apiKey} />}
                            {view === 'chat' && <ChatMode />}
                            {view === 'settings' && <Settings apiKey={apiKey} setApiKey={setApiKey} />}
                        </main>
                        {menuOpen && (
                            <div className="absolute inset-0 z-50 flex">
                                <div className="absolute inset-0 bg-black/20" onClick={() => setMenuOpen(false)}></div>
                                <div className="relative w-64 bg-white h-full shadow-2xl animate-slide-up flex flex-col p-4">
                                    <div className="flex justify-end border-b pb-4 mb-4"><button onClick={() => setMenuOpen(false)}><Icons.X /></button></div>
                                    <div className="space-y-4">
                                        <button onClick={() => {setView('translator'); setMenuOpen(false);}} className="w-full text-left font-bold text-gray-700 py-2">🇹🇭 翻译神器</button>
                                        <button onClick={() => {setView('chat'); setMenuOpen(false);}} className="w-full text-left font-bold text-gray-700 py-2">💬 爱豆聊天</button>
                                        <button onClick={() => {setView('settings'); setMenuOpen(false);}} className="w-full text-left font-bold text-gray-700 py-2">⚙️ 填写 API Key</button>
                                    </div>
                                </div>
                            </div>
                        )}
                    </div>
                </ToastProvider>
            );
        };

        const Translator = ({ apiKey }) => {
            const [input, setInput] = useState('');
            const [result, setResult] = useState(null);
            const [loading, setLoading] = useState(false);
            const showToast = React.useContext(ToastContext);

            const handleTranslate = async () => {
                if (!input.trim()) return;
                if (!apiKey) return showToast('请先在菜单中设置 Gemini API Key', 'error');
                setLoading(true);
                try {
                    const res = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent?key=${apiKey}`, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify({ contents: [{ parts: [{ text: `Translate "${input}" to Thai or Chinese. Return JSON: {"translated":"..","phonetic":"..","segments":[{"word":"..","meaning":".."}]}` }] }] })
                    });
                    const data = await res.json();
                    let text = data.candidates[0].content.parts[0].text.replace(/```json/g, '').replace(/```/g, '').trim();
                    setResult(JSON.parse(text));
                } catch (e) {
                    showToast('翻译失败，请检查网络或 Key', 'error');
                } finally {
                    setLoading(false);
                }
            };

            return (
                <div className="flex flex-col gap-6">
                    <div className="glass-panel rounded-2xl p-4 shadow-sm">
                        <textarea value={input} onChange={(e) => setInput(e.target.value)} placeholder="输入中文或泰语..." className="w-full bg-transparent outline-none resize-none h-24 text-lg text-gray-700" />
                        <div className="flex justify-end mt-2"><button onClick={handleTranslate} className="bg-ruai-dark-blue text-white px-6 py-2 rounded-full font-bold btn-bounce">{loading ? '...' : '翻译'}</button></div>
                    </div>
                    {result && (
                        <div className="glass-panel rounded-2xl p-5 border-l-4 border-ruai-accent bg-white">
                            <h2 className="text-2xl font-bold text-gray-800 mb-2">{result.translated}</h2>
                            <p className="text-sm text-gray-500 font-mono mb-4">{result.phonetic}</p>
                            <div className="flex flex-wrap gap-2">
                                {result.segments?.map((seg, i) => <div key={i} className="bg-blue-50 px-3 py-1 rounded-lg text-sm"><span className="font-bold text-blue-800">{seg.word}</span> <span className="text-xs text-blue-500">{seg.meaning}</span></div>)}
                            </div>
                        </div>
                    )}
                </div>
            );
        };

        const ChatMode = () => {
            const [msgs, setMsgs] = useState([{ sender: 'idol', text: 'Sawasdee krub! Kid tueng pom mai?' }]);
            const [input, setInput] = useState('');
            const send = () => {
                if(!input) return;
                setMsgs(prev => [...prev, {sender: 'me', text: input}]);
                setInput('');
                setTimeout(() => setMsgs(prev => [...prev, {sender: 'idol', text: '5555 narak mak krub!'}]), 1000);
            };
            return (
                <div className="flex flex-col h-[75vh]">
                    <div className="flex-1 overflow-y-auto space-y-4 pb-4">
                        {msgs.map((m, i) => (
                            <div key={i} className={`flex ${m.sender === 'me' ? 'justify-end' : 'justify-start'}`}>
                                <div className={`max-w-[75%] p-3 rounded-2xl text-sm ${m.sender === 'me' ? 'bg-ruai-dark-blue text-white rounded-tr-none' : 'bg-white text-gray-800 rounded-tl-none'}`}>{m.text}</div>
                            </div>
                        ))}
                    </div>
                    <div className="bg-white p-2 rounded-full shadow-lg flex items-center gap-2">
                        <input value={input} onChange={e=>setInput(e.target.value)} placeholder="To P'Idol..." className="flex-1 bg-transparent outline-none text-sm px-2" />
                        <button onClick={send} className="p-2 bg-ruai-accent text-white rounded-full btn-bounce"><Icons.Send /></button>
                    </div>
                </div>
            );
        };

        const Settings = ({ apiKey, setApiKey }) => {
            const [val, setVal] = useState(apiKey);
            const showToast = React.useContext(ToastContext);
            return (
                <div className="glass-panel p-6 rounded-2xl">
                    <h2 className="text-xl font-bold mb-4">设置 Gemini API Key</h2>
                    <input type="password" value={val} onChange={e=>setVal(e.target.value)} className="w-full p-3 rounded-lg border mb-4" placeholder="AIzaSy..." />
                    <button onClick={() => { setApiKey(val); showToast('已保存'); }} className="w-full bg-ruai-dark-blue text-white py-3 rounded-xl font-bold">保存</button>
                </div>
            );
        };

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
