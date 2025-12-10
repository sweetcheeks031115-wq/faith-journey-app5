# faith-journey-app5
God the devine 
import React, { useState, useEffect, useRef } from 'react';
import { BookOpen, MessageCircle, Book, Heart, Send, Plus, Search, CheckCircle, Circle } from 'lucide-react';

const BibleApp = () => {
  const [activeTab, setActiveTab] = useState('home');
  const [chatMessages, setChatMessages] = useState([]);
  const [chatInput, setChatInput] = useState('');
  const [journalEntries, setJournalEntries] = useState([]);
  const [newEntry, setNewEntry] = useState('');
  const [isLoading, setIsLoading] = useState(false);
  const [deepStudyVerse, setDeepStudyVerse] = useState('');
  const [deepStudyResult, setDeepStudyResult] = useState('');
  const [isStudying, setIsStudying] = useState(false);
  const [selectedPlan, setSelectedPlan] = useState(null);
  const [planProgress, setPlanProgress] = useState({});
  const chatEndRef = useRef(null);

  const dailyPrayer = {
    title: "Morning Prayer for Guidance",
    text: "Heavenly Father, as I begin this new day, I come before You with a grateful heart. Thank You for the gift of life and the breath in my lungs. Guide my steps today, Lord. Give me wisdom in my decisions, strength in my challenges, and peace in my heart. Help me to see Your hand in all things and to trust in Your perfect plan for my life. May my words and actions reflect Your love. In Jesus' name, Amen."
  };

  const dailyVerse = {
    reference: "Jeremiah 29:11",
    text: "For I know the plans I have for you, declares the Lord, plans to prosper you and not to harm you, plans to give you hope and a future.",
    explanation: "This powerful promise reminds us that God has a specific plan for each of our lives. Even when we can't see the path ahead, He is working everything together for our good. His plans are filled with hope, not harm. Trust that He knows what's best for you."
  };

  const topics = [
    { 
      name: "Finding Your Purpose", 
      icon: "🎯",
      verses: [
        { ref: "Ephesians 2:10", text: "For we are God's handiwork, created in Christ Jesus to do good works, which God prepared in advance for us to do." },
        { ref: "Proverbs 19:21", text: "Many are the plans in a person's heart, but it is the Lord's purpose that prevails." }
      ]
    },
    { 
      name: "God's Promises", 
      icon: "✨",
      verses: [
        { ref: "Isaiah 41:10", text: "So do not fear, for I am with you; do not be dismayed, for I am your God." },
        { ref: "Philippians 4:19", text: "And my God will meet all your needs according to the riches of his glory in Christ Jesus." }
      ]
    },
    { 
      name: "Strength & Courage", 
      icon: "💪",
      verses: [
        { ref: "Joshua 1:9", text: "Be strong and courageous. Do not be afraid; do not be discouraged, for the Lord your God will be with you wherever you go." },
        { ref: "Philippians 4:13", text: "I can do all this through him who gives me strength." }
      ]
    },
    { 
      name: "Peace & Hope", 
      icon: "🕊️",
      verses: [
        { ref: "John 14:27", text: "Peace I leave with you; my peace I give you. I do not give to you as the world gives. Do not let your hearts be troubled and do not be afraid." },
        { ref: "Romans 15:13", text: "May the God of hope fill you with all joy and peace as you trust in him." }
      ]
    }
  ];

  const readingPlans = [
    {
      id: 'psalms-proverbs',
      name: '30-Day Psalms & Proverbs',
      description: 'Read one Psalm and one Proverbs chapter each day',
      duration: '30 days',
      icon: '📜',
      days: Array.from({ length: 30 }, (_, i) => ({
        day: i + 1,
        reading: `Psalm ${i + 1} & Proverbs ${i + 1}`
      }))
    },
    {
      id: 'gospels',
      name: 'Gospels in 40 Days',
      description: 'Journey through the life of Jesus',
      duration: '40 days',
      icon: '✝️',
      days: [
        ...Array.from({ length: 10 }, (_, i) => ({ day: i + 1, reading: `Matthew ${i + 1}-${i + 3}` })),
        ...Array.from({ length: 10 }, (_, i) => ({ day: i + 11, reading: `Mark ${i + 1}-${i + 2}` })),
        ...Array.from({ length: 12 }, (_, i) => ({ day: i + 21, reading: `Luke ${i + 1}-${i + 2}` })),
        ...Array.from({ length: 8 }, (_, i) => ({ day: i + 33, reading: `John ${i + 1}-${i + 3}` }))
      ]
    },
    {
      id: 'faith-foundations',
      name: 'Faith Foundations',
      description: 'Essential verses for new believers',
      duration: '21 days',
      icon: '🌟',
      days: [
        { day: 1, reading: 'John 3:16-21 (Salvation)' },
        { day: 2, reading: 'Romans 3:23-26 (Sin & Grace)' },
        { day: 3, reading: 'Ephesians 2:8-10 (Saved by Faith)' },
        { day: 4, reading: 'John 14:1-6 (Jesus is the Way)' },
        { day: 5, reading: 'Romans 8:1-11 (Life in the Spirit)' },
        { day: 6, reading: 'Matthew 6:9-13 (The Lord\'s Prayer)' },
        { day: 7, reading: 'Psalm 23 (The Good Shepherd)' },
        { day: 8, reading: '1 Corinthians 13 (Love)' },
        { day: 9, reading: 'Matthew 5:1-12 (Beatitudes)' },
        { day: 10, reading: 'Romans 12:1-8 (Living Sacrifice)' },
        { day: 11, reading: 'Philippians 4:4-13 (Joy & Peace)' },
        { day: 12, reading: 'James 1:2-8 (Faith & Trials)' },
        { day: 13, reading: 'Hebrews 11:1-6 (Faith Defined)' },
        { day: 14, reading: 'Matthew 28:18-20 (Great Commission)' },
        { day: 15, reading: 'Galatians 5:22-26 (Fruit of Spirit)' },
        { day: 16, reading: 'Psalm 119:9-16 (God\'s Word)' },
        { day: 17, reading: '1 John 4:7-21 (God is Love)' },
        { day: 18, reading: 'Romans 8:28-39 (Nothing Separates)' },
        { day: 19, reading: 'Jeremiah 29:11-13 (God\'s Plans)' },
        { day: 20, reading: 'Isaiah 40:28-31 (Strength Renewed)' },
        { day: 21, reading: 'Revelation 21:1-7 (New Creation)' }
      ]
    }
  ];

  useEffect(() => {
    chatEndRef.current?.scrollIntoView({ behavior: 'smooth' });
  }, [chatMessages]);

  const handleChatSubmit = async (e) => {
    e.preventDefault();
    if (!chatInput.trim()) return;

    const userMessage = chatInput.trim();
    setChatMessages(prev => [...prev, { role: 'user', content: userMessage }]);
    setChatInput('');
    setIsLoading(true);

    try {
      const response = await fetch('https://api.anthropic.com/v1/messages', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          model: 'claude-sonnet-4-20250514',
          max_tokens: 1000,
          messages: [
            {
              role: 'user',
              content: `You are a compassionate Christian spiritual guide helping someone learn about the Bible and grow in their faith. They are a beginner seeking guidance, encouragement, and understanding of God's word. Keep responses warm, supportive, and grounded in scripture. When sharing verses, include the reference. Keep responses concise but meaningful.

User's question: ${userMessage}`
            }
          ]
        })
      });

      const data = await response.json();
      const assistantMessage = data.content
        .filter(item => item.type === 'text')
        .map(item => item.text)
        .join('\n');

      setChatMessages(prev => [...prev, { role: 'assistant', content: assistantMessage }]);
    } catch (error) {
      setChatMessages(prev => [...prev, { 
        role: 'assistant', 
        content: 'I apologize, but I encountered an error. Please try again.' 
      }]);
    } finally {
      setIsLoading(false);
    }
  };

  const addJournalEntry = () => {
    if (!newEntry.trim()) return;
    
    const entry = {
      id: Date.now(),
      date: new Date().toLocaleDateString('en-US', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' }),
      content: newEntry.trim()
    };
    
    setJournalEntries(prev => [entry, ...prev]);
    setNewEntry('');
  };

  const handleDeepStudy = async () => {
    if (!deepStudyVerse.trim()) return;

    setIsStudying(true);
    setDeepStudyResult('');

    try {
      const response = await fetch('https://api.anthropic.com/v1/messages', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          model: 'claude-sonnet-4-20250514',
          max_tokens: 1500,
          messages: [
            {
              role: 'user',
              content: `You are a knowledgeable Bible teacher helping someone deeply understand scripture. The user wants to study this verse: "${deepStudyVerse}"

Provide a comprehensive study that includes:
1. The full verse text (if they provided just a reference)
2. Historical and cultural context
3. What it meant to the original audience
4. Key themes and theological significance
5. How it applies to life today
6. Cross-references to related scriptures

Keep the explanation accessible for beginners but thorough and insightful.`
            }
          ]
        })
      });

      const data = await response.json();
      const result = data.content
        .filter(item => item.type === 'text')
        .map(item => item.text)
        .join('\n');

      setDeepStudyResult(result);
    } catch (error) {
      setDeepStudyResult('I apologize, but I encountered an error. Please try again.');
    } finally {
      setIsStudying(false);
    }
  };

  const toggleDayComplete = (planId, day) => {
    setPlanProgress(prev => {
      const key = `${planId}-${day}`;
      const newProgress = { ...prev };
      if (newProgress[key]) {
        delete newProgress[key];
      } else {
        newProgress[key] = true;
      }
      return newProgress;
    });
  };

  const getPlanProgress = (planId, totalDays) => {
    const completed = Object.keys(planProgress).filter(key => 
      key.startsWith(`${planId}-`) && planProgress[key]
    ).length;
    return Math.round((completed / totalDays) * 100);
  };

  const renderHome = () => (
    <div className="space-y-6 pb-6">
      <div className="bg-gradient-to-br from-pink-100 via-rose-100 to-orange-100 rounded-2xl p-6 shadow-md">
        <div className="flex items-center gap-2 mb-3">
          <span className="text-2xl">🙏</span>
          <h2 className="text-xl font-semibold text-gray-800">Morning Prayer</h2>
        </div>
        <h3 className="font-medium text-gray-700 mb-3">{dailyPrayer.title}</h3>
        <p className="text-gray-600 leading-relaxed italic">{dailyPrayer.text}</p>
      </div>

      <div className="bg-gradient-to-br from-yellow-100 via-amber-100 to-orange-100 rounded-2xl p-6 shadow-md">
        <div className="flex items-center gap-2 mb-3">
          <BookOpen className="text-orange-600" size={24} />
          <h2 className="text-xl font-semibold text-gray-800">Today's Verse</h2>
        </div>
        <p className="text-sm font-medium text-orange-700 mb-2">{dailyVerse.reference}</p>
        <p className="text-gray-700 mb-4 font-medium leading-relaxed">"{dailyVerse.text}"</p>
        <div className="bg-white/70 rounded-lg p-4">
          <p className="text-sm text-gray-600 leading-relaxed">{dailyVerse.explanation}</p>
        </div>
      </div>

      <div className="grid grid-cols-2 gap-3">
        <button 
          onClick={() => setActiveTab('chat')}
          className="bg-gradient-to-br from-pink-200 to-rose-200 rounded-xl p-4 shadow-md hover:shadow-lg transition-shadow border border-pink-300"
        >
          <MessageCircle className="text-rose-700 mb-2" size={28} />
          <p className="font-medium text-gray-800 text-sm">Chat Assistant</p>
        </button>
        <button 
          onClick={() => setActiveTab('journal')}
          className="bg-gradient-to-br from-yellow-200 to-amber-200 rounded-xl p-4 shadow-md hover:shadow-lg transition-shadow border border-yellow-300"
        >
          <Heart className="text-amber-700 mb-2" size={28} />
          <p className="font-medium text-gray-800 text-sm">Gratitude Journal</p>
        </button>
      </div>
    </div>
  );

  const renderChat = () => (
    <div className="flex flex-col h-[calc(100vh-12rem)]">
      <div className="bg-gradient-to-r from-pink-200 via-rose-200 to-orange-200 rounded-t-2xl p-4 border-b border-rose-300">
        <div className="flex items-center gap-2">
          <MessageCircle className="text-rose-700" size={24} />
          <h2 className="text-xl font-semibold text-gray-800">Spiritual Guide</h2>
        </div>
        <p className="text-sm text-gray-700 mt-1">Ask questions about faith, scripture, or life</p>
      </div>
      
      <div className="flex-1 overflow-y-auto p-4 space-y-4 bg-gradient-to-br from-pink-50 to-orange-50">
        {chatMessages.length === 0 ? (
          <div className="text-center py-12">
            <MessageCircle className="mx-auto text-rose-300 mb-4" size={48} />
            <p className="text-gray-600 mb-2 font-medium">How can I help you today?</p>
            <p className="text-sm text-gray-500">Ask about scripture, finding purpose, or guidance</p>
          </div>
        ) : (
          chatMessages.map((msg, idx) => (
            <div key={idx} className={`flex ${msg.role === 'user' ? 'justify-end' : 'justify-start'}`}>
              <div className={`max-w-[80%] rounded-2xl px-4 py-3 shadow-sm ${
                msg.role === 'user' 
                  ? 'bg-gradient-to-br from-rose-500 to-pink-500 text-white' 
                  : 'bg-white text-gray-800 border border-pink-100'
              }`}>
                <p className="text-sm leading-relaxed whitespace-pre-wrap">{msg.content}</p>
              </div>
            </div>
          ))
        )}
        {isLoading && (
          <div className="flex justify-start">
            <div className="bg-white border border-pink-100 rounded-2xl px-4 py-3 shadow-sm">
              <p className="text-sm text-gray-600">Thinking...</p>
            </div>
          </div>
        )}
        <div ref={chatEndRef} />
      </div>
      
      <div className="p-4 bg-gradient-to-r from-pink-100 to-rose-100 border-t border-rose-200 rounded-b-2xl">
        <div className="flex gap-2">
          <input
            type="text"
            value={chatInput}
            onChange={(e) => setChatInput(e.target.value)}
            onKeyPress={(e) => e.key === 'Enter' && !e.shiftKey && handleChatSubmit(e)}
            placeholder="Ask your question..."
            className="flex-1 px-4 py-3 rounded-xl border-2 border-rose-300 focus:outline-none focus:ring-2 focus:ring-rose-400 bg-white"
            disabled={isLoading}
          />
          <button
            onClick={handleChatSubmit}
            disabled={isLoading || !chatInput.trim()}
            className="bg-gradient-to-br from-rose-500 to-pink-500 text-white rounded-xl px-6 hover:from-rose-600 hover:to-pink-600 disabled:from-gray-300 disabled:to-gray-400 disabled:cursor-not-allowed transition-all shadow-md"
          >
            <Send size={20} />
          </button>
        </div>
      </div>
    </div>
  );

  const renderTopics = () => (
    <div className="space-y-4 pb-6">
      <div className="bg-gradient-to-r from-purple-200 via-pink-200 to-rose-200 rounded-2xl p-4 mb-4 shadow-md">
        <div className="flex items-center gap-2">
          <Book className="text-purple-700" size={24} />
          <h2 className="text-xl font-semibold text-gray-800">Learn by Topic</h2>
        </div>
        <p className="text-sm text-gray-700 mt-1">Explore God's word organized by theme</p>
      </div>

      <div className="bg-gradient-to-br from-indigo-100 via-purple-100 to-pink-100 rounded-xl p-5 shadow-md border border-purple-200 mb-4">
        <div className="flex items-center gap-2 mb-3">
          <Search className="text-indigo-700" size={22} />
          <h3 className="font-semibold text-gray-800">Deep Study a Verse</h3>
        </div>
        <p className="text-sm text-gray-600 mb-3">Enter any Bible verse or reference for an in-depth study</p>
        <input
          type="text"
          value={deepStudyVerse}
          onChange={(e) => setDeepStudyVerse(e.target.value)}
          placeholder="e.g., John 3:16 or Philippians 4:13"
          className="w-full px-4 py-2 rounded-lg border-2 border-purple-300 focus:outline-none focus:ring-2 focus:ring-purple-400 bg-white mb-3"
        />
        <button
          onClick={handleDeepStudy}
          disabled={isStudying || !deepStudyVerse.trim()}
          className="w-full bg-gradient-to-br from-indigo-500 to-purple-500 text-white rounded-lg py-2 font-medium hover:from-indigo-600 hover:to-purple-600 disabled:from-gray-300 disabled:to-gray-400 disabled:cursor-not-allowed transition-all shadow-md flex items-center justify-center gap-2"
        >
          <Search size={18} />
          {isStudying ? 'Studying...' : 'Deep Study'}
        </button>

        {deepStudyResult && (
          <div className="mt-4 bg-white/80 rounded-lg p-4 border border-purple-200">
            <p className="text-sm text-gray-700 leading-relaxed whitespace-pre-wrap">{deepStudyResult}</p>
          </div>
        )}
      </div>

      {topics.map((topic, idx) => (
        <div key={idx} className="bg-gradient-to-br from-white to-pink-50 rounded-xl p-5 shadow-md border border-pink-200">
          <div className="flex items-center gap-2 mb-4">
            <span className="text-2xl">{topic.icon}</span>
            <h3 className="font-semibold text-gray-800">{topic.name}</h3>
          </div>
          <div className="space-y-3">
            {topic.verses.map((verse, vIdx) => (
              <div key={vIdx} className="bg-gradient-to-br from-yellow-50 to-orange-50 rounded-lg p-3 border border-yellow-200">
                <p className="text-xs font-medium text-orange-700 mb-1">{verse.ref}</p>
                <p className="text-sm text-gray-700 leading-relaxed">"{verse.text}"</p>
                <button
                  onClick={() => {
                    setDeepStudyVerse(verse.ref);
                    window.scrollTo({ top: 0, behavior: 'smooth' });
                  }}
                  className="mt-2 text-xs text-purple-600 hover:text-purple-700 font-medium flex items-center gap-1"
                >
                  <Search size={14} />
                  Deep Study
                </button>
              </div>
            ))}
          </div>
        </div>
      ))}
    </div>
  );

  const renderJournal = () => (
    <div className="space-y-4 pb-6">
      <div className="bg-gradient-to-r from-yellow-200 via-amber-200 to-orange-200 rounded-2xl p-4 shadow-md">
        <div className="flex items-center gap-2">
          <Heart className="text-orange-700" size={24} />
          <h2 className="text-xl font-semibold text-gray-800">Gratitude Journal</h2>
        </div>
        <p className="text-sm text-gray-700 mt-1">Record your blessings and answered prayers</p>
      </div>

      <div className="bg-gradient-to-br from-white to-yellow-50 rounded-xl p-4 shadow-md border border-yellow-200">
        <textarea
          value={newEntry}
          onChange={(e) => setNewEntry(e.target.value)}
          placeholder="What are you grateful for today?"
          className="w-full px-3 py-2 rounded-lg border-2 border-yellow-300 focus:outline-none focus:ring-2 focus:ring-amber-400 resize-none bg-white"
          rows="4"
        />
        <button
          onClick={addJournalEntry}
          disabled={!newEntry.trim()}
          className="mt-3 w-full bg-gradient-to-br from-amber-500 to-orange-500 text-white rounded-lg py-2 font-medium hover:from-amber-600 hover:to-orange-600 disabled:from-gray-300 disabled:to-gray-400 disabled:cursor-not-allowed transition-all flex items-center justify-center gap-2 shadow-md"
        >
          <Plus size={20} />
          Add Entry
        </button>
      </div>

      <div className="space-y-3">
        {journalEntries.length === 0 ? (
          <div className="text-center py-12 bg-gradient-to-br from-white to-pink-50 rounded-xl shadow-md border border-pink-200">
            <Heart className="mx-auto text-pink-3