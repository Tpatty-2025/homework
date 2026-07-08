import React, { useState, useEffect } from 'react';
import { Calendar, Edit3, BookOpen, Clock, Copy, CheckCircle2, X, Plus, Lock, Unlock, Code } from 'lucide-react';

// 預設的初始資料（未來您在 GitHub 上就是直接替換這段資料）
const INITIAL_DATA = [
  {
    date: '2026-07-08',
    quote: '充滿活力迎接每一天，勇敢探索新事物！',
    homework: [
      '1. 國語習作 P.12 - P.14',
      '2. 數學考卷一張 (訂正並簽名)',
      '3. 閱讀課外讀物 30 分鐘'
    ],
    reminders: '明天有體育課，請記得穿著運動服與運動鞋。'
  }
];

const getTodayString = () => {
  const today = new Date();
  return today.toISOString().split('T')[0];
};

const formatDateToChinese = (dateString) => {
  const date = new Date(dateString);
  const days = ['日', '一', '二', '三', '四', '五', '六'];
  return `${date.getMonth() + 1}月${date.getDate()}日 (星期${days[date.getDay()]})`;
};

export default function ContactBook() {
  // 初始化資料：優先讀取瀏覽器暫存，若無則使用初始資料
  const [entries, setEntries] = useState(() => {
    try {
      const saved = localStorage.getItem('schoolContactBook');
      if (saved) return JSON.parse(saved);
    } catch (e) {
      console.error('Failed to load from localStorage');
    }
    return INITIAL_DATA;
  });

  const [selectedDate, setSelectedDate] = useState('2026-07-08');
  const [viewMode, setViewMode] = useState('student');
  const [showToast, setShowToast] = useState(false);
  const [toastMessage, setToastMessage] = useState('');
  const [isSidebarOpen, setIsSidebarOpen] = useState(false);
  const [isAuthenticated, setIsAuthenticated] = useState(false);
  const [showAuthModal, setShowAuthModal] = useState(false);
  const [password, setPassword] = useState('');
  const [passwordError, setPasswordError] = useState('');

  const [editForm, setEditForm] = useState({
    quote: '',
    homework: '',
    reminders: ''
  });

  const currentEntry = entries.find(e => e.date === selectedDate) || {
    date: selectedDate,
    quote: entries.length > 0 ? entries[0].quote : '充滿活力迎接每一天！',
    homework: [],
    reminders: ''
  };

  useEffect(() => {
    if (viewMode === 'teacher') {
      setEditForm({
        quote: currentEntry.quote || '',
        homework: (currentEntry.homework || []).join('\n'),
        reminders: currentEntry.reminders || ''
      });
    }
  }, [selectedDate, viewMode, currentEntry]);

  // 儲存至瀏覽器本地端
  const handleSaveToLocal = () => {
    const newHomework = editForm.homework
      .split('\n')
      .map(line => line.trim())
      .filter(line => line !== '');

    const updatedEntry = {
      date: selectedDate,
      quote: editForm.quote,
      homework: newHomework,
      reminders: editForm.reminders
    };

    let newEntries = [...entries];
    const existingIndex = entries.findIndex(e => e.date === selectedDate);
    
    if (existingIndex >= 0) {
      newEntries[existingIndex] = updatedEntry;
    } else {
      newEntries.push(updatedEntry);
      newEntries.sort((a, b) => new Date(b.date) - new Date(a.date));
    }

    setEntries(newEntries);
    localStorage.setItem('schoolContactBook', JSON.stringify(newEntries));
    showNotification('✅ 已暫存於本機！若要發布請記得匯出至 GitHub。');
  };

  // 匯出資料給 GitHub 使用
  const handleExportForGithub = () => {
    const dataString = JSON.stringify(entries, null, 2);
    const exportText = `// 請將以下內容覆蓋 GitHub 上的 INITIAL_DATA\nconst INITIAL_DATA = ${dataString};`;
    
    try {
      const textArea = document.createElement("textarea");
      textArea.value = exportText;
      document.body.appendChild(textArea);
      textArea.select();
      document.execCommand("copy");
      document.body.removeChild(textArea);
      showNotification('🚀 程式碼已複製！請前往 GitHub 貼上更新。');
    } catch (err) {
      console.error('Copy failed', err);
    }
  };

  const showNotification = (msg) => {
    setToastMessage(msg);
    setShowToast(true);
    setTimeout(() => setShowToast(false), 4000);
  };

  const handleCopy = () => {
    const textToCopy = `【${formatDateToChinese(currentEntry.date)} 聯絡簿】\n\n` +
      `🌟 每週一句：${currentEntry.quote}\n\n` +
      `📝 今日任務：\n${(currentEntry.homework || []).join('\n')}\n\n` +
      `🔔 溫馨提醒：\n${currentEntry.reminders}`;

    try {
      const textArea = document.createElement("textarea");
      textArea.value = textToCopy;
      document.body.appendChild(textArea);
      textArea.select();
      document.execCommand("copy");
      document.body.removeChild(textArea);
      showNotification('📋 已複製聯絡簿內容！');
    } catch (err) {
      console.error('Copy failed', err);
    }
  };

  const handleLogin = (e) => {
    e.preventDefault();
    if (password === 'St113') {
      setIsAuthenticated(true);
      setShowAuthModal(false);
      setViewMode('teacher');
      showNotification('🔓 已進入教師管理模式');
    } else {
      setPasswordError('密碼錯誤');
    }
  };

  const renderSidebar = () => (
    <div className={`fixed inset-y-0 left-0 transform ${isSidebarOpen ? 'translate-x-0' : '-translate-x-full'} md:relative md:translate-x-0 z-20 w-64 bg-white border-r border-gray-100 shadow-xl md:shadow-none transition-transform duration-300 flex flex-col h-full`}>
      <div className="p-4 border-b border-amber-100 flex justify-between items-center bg-amber-50">
        <div className="flex items-center text-amber-800 font-bold text-lg tracking-wide">
          <Clock className="w-5 h-5 mr-2" />
          聯絡簿歷史
        </div>
        <button className="md:hidden text-gray-400 hover:text-gray-600" onClick={() => setIsSidebarOpen(false)}>
          <X className="w-6 h-6" />
        </button>
      </div>
      
      <div className="flex-1 overflow-y-auto p-3 space-y-2">
        {isAuthenticated && (
          <button 
            onClick={() => {
              setSelectedDate(getTodayString());
              setViewMode('teacher');
              setIsSidebarOpen(false);
            }}
            className="w-full flex items-center justify-center py-3 px-4 border-2 border-amber-400 rounded-xl text-amber-700 bg-amber-50 hover:bg-amber-100 font-bold mb-4 transition-colors shadow-sm"
          >
            <Plus className="w-5 h-5 mr-1" /> 新增本日內容
          </button>
        )}

        {entries.map(entry => (
          <button
            key={entry.date}
            onClick={() => {
              setSelectedDate(entry.date);
              setViewMode('student');
              setIsSidebarOpen(false);
            }}
            className={`w-full text-left px-4 py-3 rounded-xl transition-all flex items-center ${
              selectedDate === entry.date 
                ? 'bg-blue-600 text-white shadow-md font-bold' 
                : 'bg-slate-50 hover:bg-slate-100 text-slate-700'
            }`}
          >
            <Calendar className={`w-4 h-4 mr-3 ${selectedDate === entry.date ? 'text-blue-200' : 'text-slate-400'}`} />
            <span>{formatDateToChinese(entry.date)}</span>
          </button>
        ))}
      </div>
    </div>
  );

  const renderStudentView = () => (
    <div className="bg-white rounded-3xl shadow-lg overflow-hidden border border-slate-100">
      <div className="bg-gradient-to-r from-blue-600 to-blue-500 text-white p-8 relative flex justify-between items-end">
        <div>
          <h2 className="text-3xl md:text-5xl font-black tracking-tight">{formatDateToChinese(currentEntry.date)}</h2>
          <p className="mt-3 text-blue-100 text-lg flex items-center font-medium">
            <BookOpen className="w-5 h-5 mr-2" /> 班級數位聯絡簿
          </p>
        </div>
        <button 
          onClick={handleCopy}
          className="hidden md:flex items-center bg-white/20 hover:bg-white/30 px-5 py-2.5 rounded-full text-sm font-bold backdrop-blur-sm transition-all"
        >
          <Copy className="w-4 h-4 mr-2" /> 複製文字
        </button>
      </div>

      <div className="p-8 md:p-12 space-y-8 text-slate-800 text-lg md:text-xl">
        {currentEntry.quote && (
          <div className="flex items-start bg-amber-50 p-6 rounded-2xl border border-amber-100">
            <span className="font-black text-amber-600 w-28 shrink-0">🌟 本週金句</span>
            <span className="text-slate-700 font-medium">{currentEntry.quote}</span>
          </div>
        )}

        <div className="flex items-start">
          <span className="font-black text-blue-600 w-28 shrink-0 pt-1">📝 今日任務</span>
          <div className="flex-1 space-y-3">
            {currentEntry.homework && currentEntry.homework.length > 0 ? (
              currentEntry.homework.map((hw, idx) => (
                <div key={idx} className="font-medium text-slate-700 bg-slate-50 p-3 rounded-lg border border-slate-100">{hw}</div>
              ))
            ) : (
              <div className="text-slate-400 italic">本日無指定任務</div>
            )}
          </div>
        </div>

        <div className="flex items-start">
          <span className="font-black text-rose-500 w-28 shrink-0 pt-1">🔔 溫馨提醒</span>
          <div className="flex-1">
            {currentEntry.reminders ? (
              <div className="font-medium text-slate-700 bg-rose-50 p-4 rounded-xl border border-rose-100 whitespace-pre-line">
                {currentEntry.reminders}
              </div>
            ) : (
              <div className="text-slate-400 italic">無特別注意事項</div>
            )}
          </div>
        </div>
      </div>
    </div>
  );

  const renderTeacherView = () => (
    <div className="bg-white rounded-3xl shadow-lg overflow-hidden border border-slate-200 flex flex-col">
      <div className="bg-slate-800 text-white p-6 flex justify-between items-center">
        <div>
          <h2 className="text-2xl font-bold flex items-center">
            <Edit3 className="w-6 h-6 mr-3 text-amber-400" />
            後台編輯 - {formatDateToChinese(selectedDate)}
          </h2>
          <p className="text-slate-400 mt-1 text-sm font-medium">資料存檔後，需匯出並更新至 GitHub 才會公開生效</p>
        </div>
        <button 
          onClick={handleExportForGithub}
          className="bg-amber-500 hover:bg-amber-400 text-slate-900 px-4 py-2 rounded-lg font-bold flex items-center transition-colors shadow-sm"
        >
          <Code className="w-4 h-4 mr-2" /> 匯出 GitHub 資料
        </button>
      </div>

      <div className="p-6 md:p-8 space-y-6 flex-1 bg-slate-50">
        <div>
          <label className="block text-sm font-bold text-slate-700 mb-2">每週一句</label>
          <input
            type="text"
            className="w-full px-4 py-3 rounded-xl border-2 border-slate-200 focus:border-blue-500 outline-none transition-colors font-medium"
            value={editForm.quote}
            onChange={(e) => setEditForm({...editForm, quote: e.target.value})}
          />
        </div>

        <div>
          <label className="block text-sm font-bold text-slate-700 mb-2">今日任務 (一行一項)</label>
          <textarea
            className="w-full px-4 py-3 rounded-xl border-2 border-slate-200 focus:border-blue-500 outline-none transition-colors h-48 resize-y font-medium leading-relaxed"
            value={editForm.homework}
            onChange={(e) => setEditForm({...editForm, homework: e.target.value})}
          />
        </div>

        <div>
          <label className="block text-sm font-bold text-slate-700 mb-2">注意事項</label>
          <textarea
            className="w-full px-4 py-3 rounded-xl border-2 border-slate-200 focus:border-blue-500 outline-none transition-colors h-32 resize-y font-medium leading-relaxed"
            value={editForm.reminders}
            onChange={(e) => setEditForm({...editForm, reminders: e.target.value})}
          />
        </div>
      </div>

      <div className="p-4 bg-white border-t border-slate-200 flex justify-end space-x-3">
        <button 
          onClick={() => setViewMode('student')}
          className="px-6 py-3 rounded-xl font-bold text-slate-500 hover:bg-slate-100 transition-colors"
        >
          返回檢視
        </button>
        <button 
          onClick={handleSaveToLocal}
          className="px-6 py-3 rounded-xl font-bold bg-blue-600 text-white hover:bg-blue-700 transition-colors shadow-md shadow-blue-200"
        >
          暫存於本機
        </button>
      </div>
    </div>
  );

  return (
    <div className="min-h-screen bg-slate-100 font-sans flex text-slate-900 selection:bg-amber-200">
      {showToast && (
        <div className="fixed top-6 left-1/2 transform -translate-x-1/2 z-50 bg-slate-900 text-white px-6 py-3.5 rounded-2xl shadow-2xl flex items-center font-bold">
          {toastMessage}
        </div>
      )}

      {showAuthModal && (
        <div className="fixed inset-0 bg-slate-900/60 z-50 flex items-center justify-center p-4 backdrop-blur-sm">
          <div className="bg-white rounded-3xl shadow-2xl w-full max-w-sm overflow-hidden">
            <div className="bg-blue-600 p-5 text-white flex justify-between items-center">
              <h3 className="font-bold text-lg flex items-center">
                <Lock className="w-5 h-5 mr-2" /> 教師登入
              </h3>
              <button onClick={() => setShowAuthModal(false)} className="text-blue-200 hover:text-white">
                <X className="w-6 h-6" />
              </button>
            </div>
            <form onSubmit={handleLogin} className="p-6 md:p-8">
              <input
                type="password"
                autoFocus
                className="w-full px-4 py-3.5 rounded-xl border-2 border-slate-200 focus:border-blue-500 outline-none transition-colors mb-2 font-medium"
                placeholder="請輸入管理密碼"
                value={password}
                onChange={(e) => {
                  setPassword(e.target.value);
                  setPasswordError('');
                }}
              />
              {passwordError && <p className="text-rose-500 text-sm font-bold mb-2">{passwordError}</p>}
              <button type="submit" className="w-full bg-blue-600 text-white font-bold py-3.5 rounded-xl hover:bg-blue-700 transition-colors mt-4 shadow-md shadow-blue-200">
                確認進入
              </button>
            </form>
          </div>
        </div>
      )}

      {renderSidebar()}

      <div className="flex-1 flex flex-col h-screen overflow-hidden">
        <header className="bg-white shadow-sm border-b border-slate-200 p-4 flex justify-between items-center z-10">
          <div className="flex items-center">
            <button className="md:hidden mr-3 p-2 text-slate-400 hover:bg-slate-100 rounded-lg" onClick={() => setIsSidebarOpen(true)}>
              <Calendar className="w-6 h-6" />
            </button>
            <h1 className="text-xl font-black text-slate-800 tracking-wide">🏫 班級聯絡簿</h1>
          </div>

          <div className="flex items-center">
            <button
              onClick={() => {
                if (isAuthenticated) {
                  setViewMode('teacher');
                } else {
                  setShowAuthModal(true);
                  setPassword('');
                }
              }}
              className="px-5 py-2.5 rounded-xl text-sm font-bold transition-colors flex items-center bg-amber-100 text-amber-800 hover:bg-amber-200"
            >
              {isAuthenticated ? <Unlock className="w-4 h-4 mr-2" /> : <Lock className="w-4 h-4 mr-2" />}
              {isAuthenticated ? '教師管理模式' : '老師登入'}
            </button>
            
            {isAuthenticated && (
              <button onClick={() => { setIsAuthenticated(false); setViewMode('student'); }} className="ml-4 text-sm font-bold text-slate-400 hover:text-slate-600">
                登出
              </button>
            )}
          </div>
        </header>

        <main className="flex-1 overflow-y-auto p-4 md:p-10 bg-slate-100">
          <div className="max-w-4xl mx-auto">
            {viewMode === 'student' ? renderStudentView() : renderTeacherView()}
          </div>
        </main>
      </div>
    </div>
  );
}
