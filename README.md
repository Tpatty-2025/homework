# homework
import React, { useState, useEffect } from 'react';
import { Calendar, Edit3, BookOpen, Clock, Copy, CheckCircle2, ChevronLeft, ChevronRight, Save, X, Plus, Lock, Unlock } from 'lucide-react';

const INITIAL_DATA = [
  {
    date: '2026-07-08',
    quote: '學海無涯，唯勤是岸。',
    homework: [
      '1. 國語習作 P.12 - P.14',
      '2. 數學考卷一張 (訂正並簽名)',
      '3. 背誦英文單字 Unit 3',
      '4. 閱讀課外讀物 30 分鐘'
    ],
    reminders: '明天有體育課，請記得穿運動服。\n下週二要交美術作品。'
  },
  {
    date: '2026-07-07',
    quote: '學海無涯，唯勤是岸。',
    homework: [
      '1. 國語課本 P.20 圈詞兩遍',
      '2. 數學作業簿 第五回'
    ],
    reminders: '請繳交午餐費 1500 元。'
  },
  {
    date: '2026-07-06',
    quote: '學海無涯，唯勤是岸。',
    homework: [
      '1. 社會學習單一張',
      '2. 自然準備第二章小考'
    ],
    reminders: '明天要帶彩色筆和剪刀。'
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
  const [entries, setEntries] = useState(INITIAL_DATA);
  const [selectedDate, setSelectedDate] = useState('2026-07-08');
  const [viewMode, setViewMode] = useState('student'); // 'student' or 'teacher'
  const [showToast, setShowToast] = useState(false);
  const [toastMessage, setToastMessage] = useState('');
  const [isSidebarOpen, setIsSidebarOpen] = useState(false);

  // Auth State
  const [isAuthenticated, setIsAuthenticated] = useState(false);
  const [showAuthModal, setShowAuthModal] = useState(false);
  const [password, setPassword] = useState('');
  const [passwordError, setPasswordError] = useState('');

  // Form state for editing
  const [editForm, setEditForm] = useState({
    quote: '',
    homework: '',
    reminders: ''
  });

  const currentEntry = entries.find(e => e.date === selectedDate) || {
    date: selectedDate,
    quote: entries.length > 0 ? entries[0].quote : '', // Default to last known quote
    homework: [],
    reminders: ''
  };

  useEffect(() => {
    // When selected date changes, populate the edit form
    if (viewMode === 'teacher') {
      setEditForm({
        quote: currentEntry.quote || '',
        homework: (currentEntry.homework || []).join('\n'),
        reminders: currentEntry.reminders || ''
      });
    }
  }, [selectedDate, viewMode, currentEntry]);

  const handleSave = () => {
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

    const existingIndex = entries.findIndex(e => e.date === selectedDate);
    let newEntries = [...entries];
    
    if (existingIndex >= 0) {
      newEntries[existingIndex] = updatedEntry;
    } else {
      newEntries.push(updatedEntry);
      // Sort descending by date
      newEntries.sort((a, b) => new Date(b.date) - new Date(a.date));
    }

    setEntries(newEntries);
    showNotification('儲存成功！');
    setViewMode('student');
  };

  const showNotification = (msg) => {
    setToastMessage(msg);
    setShowToast(true);
    setTimeout(() => setShowToast(false), 3000);
  };

  const handleCopy = () => {
    const textToCopy = `【${formatDateToChinese(currentEntry.date)} 聯絡簿】\n\n` +
      `💡 每週一句：${currentEntry.quote}\n\n` +
      `📝 作業與考試：\n${(currentEntry.homework || []).join('\n')}\n\n` +
      `⚠️ 注意事項：\n${currentEntry.reminders}`;

    try {
      const textArea = document.createElement("textarea");
      textArea.value = textToCopy;
      document.body.appendChild(textArea);
      textArea.select();
      document.execCommand("copy");
      document.body.removeChild(textArea);
      showNotification('已複製聯絡簿內容！');
    } catch (err) {
      console.error('Copy failed', err);
    }
  };

  const handleTeacherAccess = () => {
    if (isAuthenticated) {
      setViewMode('teacher');
    } else {
      setShowAuthModal(true);
      setPassword('');
      setPasswordError('');
    }
  };

  const handleLogin = (e) => {
    e.preventDefault();
   
    if (password === 'St113') {
      setIsAuthenticated(true);
      setShowAuthModal(false);
      setViewMode('teacher');
      showNotification('已成功登入教師模式');
    } else {
      setPasswordError('密碼錯誤，請重新輸入');
    }
  };

  const handleLogout = () => {
    setIsAuthenticated(false);
    setViewMode('student');
    showNotification('已登出教師模式');
  };

  const handleAddNewDay = () => {
    const today = getTodayString();
    setSelectedDate(today);
    setViewMode('teacher');
    setIsSidebarOpen(false);
  };

  const renderSidebar = () => (
    <div className={`fixed inset-y-0 left-0 transform ${isSidebarOpen ? 'translate-x-0' : '-translate-x-full'} md:relative md:translate-x-0 z-20 w-64 bg-white border-r border-gray-200 shadow-lg md:shadow-none transition-transform duration-300 ease-in-out flex flex-col h-full`}>
      <div className="p-4 border-b border-gray-200 flex justify-between items-center bg-green-50">
        <div className="flex items-center text-green-800 font-bold text-lg">
          <Clock className="w-5 h-5 mr-2" />
          歷史紀錄
        </div>
        <button className="md:hidden text-gray-500 hover:text-gray-700" onClick={() => setIsSidebarOpen(false)}>
          <X className="w-6 h-6" />
        </button>
      </div>
      
      <div className="flex-1 overflow-y-auto p-3 space-y-2">
        {/* 只有登入後才能看到新增按鈕 */}
        {isAuthenticated && (
          <button 
            onClick={handleAddNewDay}
            className="w-full flex items-center justify-center py-3 px-4 border-2 border-dashed border-green-300 rounded-lg text-green-600 hover:bg-green-50 hover:border-green-400 transition-colors font-medium mb-4"
          >
            <Plus className="w-5 h-5 mr-1" /> 新增本日聯絡簿
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
            className={`w-full text-left px-4 py-3 rounded-lg transition-all flex items-center ${
              selectedDate === entry.date 
                ? 'bg-green-600 text-white shadow-md' 
                : 'bg-gray-50 hover:bg-gray-100 text-gray-700'
            }`}
          >
            <Calendar className={`w-4 h-4 mr-3 ${selectedDate === entry.date ? 'text-green-200' : 'text-gray-400'}`} />
            <span className="font-medium">{formatDateToChinese(entry.date)}</span>
          </button>
        ))}
      </div>
    </div>
  );

  const renderStudentView = () => (
    <div className="bg-[#fdfbf7] rounded-2xl shadow-xl overflow-hidden border border-[#e0dcd3]">
      {/* Header notebook style */}
      <div className="bg-green-700 text-white p-6 relative flex justify-between items-end">
        <div className="absolute top-0 left-0 w-full h-2 bg-green-800 opacity-50"></div>
        <div>
          <h2 className="text-3xl md:text-4xl font-bold tracking-wider">{formatDateToChinese(currentEntry.date)}</h2>
          <p className="mt-2 text-green-100 text-lg flex items-center">
            <BookOpen className="w-5 h-5 mr-2" />
            班級聯絡簿
          </p>
        </div>
        <button 
          onClick={handleCopy}
          className="hidden md:flex items-center bg-white/20 hover:bg-white/30 transition-colors px-4 py-2 rounded-full text-sm font-medium backdrop-blur-sm"
        >
          <Copy className="w-4 h-4 mr-2" /> 複製文字
        </button>
      </div>

      {/* Content Area */}
      <div className="p-6 md:p-10 space-y-8 text-gray-800 bg-[linear-gradient(transparent_39px,#e5e7eb_40px)] bg-[length:100%_40px] leading-[40px] min-h-[500px]">
        
        {/* Quote of the week */}
        {currentEntry.quote && (
          <div className="flex items-start">
            <span className="font-bold text-green-700 text-xl w-32 shrink-0">每週一句：</span>
            <span className="text-xl text-gray-700">{currentEntry.quote}</span>
          </div>
        )}

        {/* Homework Items */}
        <div className="flex items-start">
          <span className="font-bold text-blue-700 text-xl w-32 shrink-0">今日作業：</span>
          <div className="flex-1">
            {currentEntry.homework && currentEntry.homework.length > 0 ? (
              currentEntry.homework.map((hw, idx) => (
                <div key={idx} className="text-xl text-gray-800">{hw}</div>
              ))
            ) : (
              <div className="text-gray-400 italic">本日無作業紀錄</div>
            )}
          </div>
        </div>

        {/* Reminders */}
        <div className="flex items-start">
          <span className="font-bold text-red-600 text-xl w-32 shrink-0">注意事項：</span>
          <div className="flex-1">
            {currentEntry.reminders ? (
              currentEntry.reminders.split('\n').map((line, idx) => (
                <div key={idx} className="text-xl text-gray-800">{line}</div>
              ))
            ) : (
              <div className="text-gray-400 italic">無特別注意事項</div>
            )}
          </div>
        </div>
      </div>

      {/* Mobile Copy Button */}
      <div className="md:hidden p-4 bg-gray-50 border-t border-gray-200">
        <button 
          onClick={handleCopy}
          className="w-full flex items-center justify-center bg-green-600 text-white px-4 py-3 rounded-lg font-medium active:bg-green-700"
        >
          <Copy className="w-5 h-5 mr-2" /> 複製聯絡簿內容
        </button>
      </div>
    </div>
  );

  const renderTeacherView = () => (
    <div className="bg-white rounded-2xl shadow-xl overflow-hidden border border-gray-200 flex flex-col">
      <div className="bg-gray-800 text-white p-6">
        <h2 className="text-2xl font-bold flex items-center">
          <Edit3 className="w-6 h-6 mr-3 text-blue-400" />
          編輯聯絡簿 - {formatDateToChinese(selectedDate)}
        </h2>
        <p className="text-gray-400 mt-1 text-sm">請在下方輸入當日的作業與注意事項</p>
      </div>

      <div className="p-6 md:p-8 space-y-6 flex-1">
        <div>
          <label className="block text-sm font-semibold text-gray-700 mb-2">每週一句</label>
          <input
            type="text"
            className="w-full px-4 py-3 rounded-lg border border-gray-300 focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none transition-all"
            placeholder="例如：學海無涯，唯勤是岸。"
            value={editForm.quote}
            onChange={(e) => setEditForm({...editForm, quote: e.target.value})}
          />
        </div>

        <div>
          <label className="block text-sm font-semibold text-gray-700 mb-2">作業與考試 (一行一項)</label>
          <textarea
            className="w-full px-4 py-3 rounded-lg border border-gray-300 focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none transition-all h-48 resize-y font-mono"
            placeholder="1. 國語習作 P.12&#10;2. 數學考卷一張"
            value={editForm.homework}
            onChange={(e) => setEditForm({...editForm, homework: e.target.value})}
          />
        </div>

        <div>
          <label className="block text-sm font-semibold text-gray-700 mb-2">注意事項</label>
          <textarea
            className="w-full px-4 py-3 rounded-lg border border-gray-300 focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none transition-all h-32 resize-y font-mono"
            placeholder="例如：明天要帶彩色筆。"
            value={editForm.reminders}
            onChange={(e) => setEditForm({...editForm, reminders: e.target.value})}
          />
        </div>
      </div>

      <div className="p-4 bg-gray-50 border-t border-gray-200 flex justify-end space-x-3">
        <button 
          onClick={() => setViewMode('student')}
          className="px-6 py-2.5 rounded-lg font-medium text-gray-600 hover:bg-gray-200 transition-colors"
        >
          取消
        </button>
        <button 
          onClick={handleSave}
          className="px-6 py-2.5 rounded-lg font-medium bg-blue-600 text-white hover:bg-blue-700 transition-colors flex items-center shadow-md shadow-blue-200"
        >
          <Save className="w-5 h-5 mr-2" /> 儲存發布
        </button>
      </div>
    </div>
  );

  return (
    <div className="min-h-screen bg-gray-100 font-sans flex text-gray-900">
      {/* Toast Notification */}
      {showToast && (
        <div className="fixed top-4 left-1/2 transform -translate-x-1/2 z-50 bg-gray-900 text-white px-6 py-3 rounded-full shadow-2xl flex items-center animate-bounce">
          <CheckCircle2 className="w-5 h-5 mr-2 text-green-400" />
          {toastMessage}
        </div>
      )}

      {/* Mobile Overlay for Sidebar */}
      {isSidebarOpen && (
        <div 
          className="fixed inset-0 bg-black/50 z-10 md:hidden transition-opacity"
          onClick={() => setIsSidebarOpen(false)}
        />
      )}

      {/* Auth Modal */}
      {showAuthModal && (
        <div className="fixed inset-0 bg-black/60 z-50 flex items-center justify-center p-4 backdrop-blur-sm">
          <div className="bg-white rounded-2xl shadow-2xl w-full max-w-sm overflow-hidden animate-in fade-in zoom-in-95 duration-200">
            <div className="bg-blue-600 p-4 text-white flex justify-between items-center">
              <h3 className="font-bold flex items-center">
                <Lock className="w-5 h-5 mr-2" /> 教師身分驗證
              </h3>
              <button onClick={() => setShowAuthModal(false)} className="text-blue-100 hover:text-white transition-colors">
                <X className="w-5 h-5" />
              </button>
            </div>
            <form onSubmit={handleLogin} className="p-6">
              <p className="text-gray-600 text-sm mb-4">請輸入教師密碼以進入編輯後台。<br/><span className="text-blue-600">(預設測試密碼為: admin)</span></p>
              <input
                type="password"
                autoFocus
                className={`w-full px-4 py-3 rounded-lg border ${passwordError ? 'border-red-500 focus:ring-red-500' : 'border-gray-300 focus:ring-blue-500'} focus:ring-2 focus:border-transparent outline-none transition-all mb-2`}
                placeholder="請輸入密碼"
                value={password}
                onChange={(e) => {
                  setPassword(e.target.value);
                  setPasswordError('');
                }}
              />
              {passwordError && <p className="text-red-500 text-sm mb-2">{passwordError}</p>}
              <button
                type="submit"
                className="w-full bg-blue-600 text-white font-medium py-3 rounded-lg hover:bg-blue-700 transition-colors mt-4"
              >
                確認登入
              </button>
            </form>
          </div>
        </div>
      )}

      {renderSidebar()}

      <div className="flex-1 flex flex-col h-screen overflow-hidden">
        {/* Top Navigation Bar */}
        <header className="bg-white shadow-sm border-b border-gray-200 p-4 flex justify-between items-center z-10">
          <div className="flex items-center">
            <button 
              className="md:hidden mr-4 p-2 -ml-2 text-gray-600 hover:bg-gray-100 rounded-lg"
              onClick={() => setIsSidebarOpen(true)}
            >
              <Calendar className="w-6 h-6" />
            </button>
            <h1 className="text-xl font-bold text-gray-800">🏫 數位聯絡簿系統</h1>
          </div>

          <div className="flex items-center">
            <div className="flex bg-gray-100 p-1 rounded-lg">
              <button
                onClick={() => setViewMode('student')}
                className={`px-4 py-2 rounded-md text-sm font-medium transition-colors ${
                  viewMode === 'student' ? 'bg-white shadow-sm text-green-700' : 'text-gray-500 hover:text-gray-700'
                }`}
              >
                學生檢視
              </button>
              <button
                onClick={handleTeacherAccess}
                className={`px-4 py-2 rounded-md text-sm font-medium transition-colors flex items-center ${
                  viewMode === 'teacher' ? 'bg-white shadow-sm text-blue-700' : 'text-gray-500 hover:text-gray-700'
                }`}
              >
                {isAuthenticated ? <Unlock className="w-4 h-4 mr-1" /> : <Lock className="w-4 h-4 mr-1" />}
                老師編輯
              </button>
            </div>
            
            {isAuthenticated && (
              <button 
                onClick={handleLogout}
                className="ml-4 text-sm font-medium text-red-500 hover:text-red-700 transition-colors hidden sm:block"
              >
                登出
              </button>
            )}
          </div>
        </header>

        {/* Main Content Scrollable Area */}
        <main className="flex-1 overflow-y-auto p-4 md:p-8 bg-gray-100">
          <div className="max-w-4xl mx-auto">
            {viewMode === 'student' ? renderStudentView() : renderTeacherView()}
          </div>
        </main>
      </div>
    </div>
  );
}
