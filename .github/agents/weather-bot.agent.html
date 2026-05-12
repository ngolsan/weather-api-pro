mport React, { useState, useEffect, useMemo } from 'react';
import { initializeApp } from 'firebase/app';
import { 
  getFirestore, 
  collection, 
  addDoc, 
  onSnapshot, 
  updateDoc, 
  doc, 
  deleteDoc, 
  query, 
  serverTimestamp 
} from 'firebase/firestore';
import { 
  getAuth, 
  signInAnonymously, 
  onAuthStateChanged,
  signInWithCustomToken 
} from 'firebase/auth';
import { 
  Plus, 
  Package, 
  Clock, 
  TrendingUp, 
  Trash2, 
  AlertCircle, 
  ChevronRight,
  Zap,
  LayoutDashboard,
  Box,
  BarChart3
} from 'lucide-react';

// --- Firebase Configuration ---
const firebaseConfig = JSON.parse(__firebase_config);
const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
const appId = typeof __app_id !== 'undefined' ? __app_id : 'flash-deal-manager';

// --- Components ---

const StatCard = ({ title, value, icon: Icon, color }) => (
  <div className="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 flex items-center gap-4">
    <div className={`p-3 rounded-xl ${color}`}>
      <Icon className="w-6 h-6 text-white" />
    </div>
    <div>
      <p className="text-sm text-slate-500 font-medium">{title}</p>
      <h3 className="text-2xl font-bold text-slate-800">{value}</h3>
    </div>
  </div>
);

const Countdown = ({ targetDate }) => {
  const [timeLeft, setTimeLeft] = useState({ h: 0, m: 0, s: 0 });

  useEffect(() => {
    const timer = setInterval(() => {
      const now = new Date().getTime();
      const distance = new Date(targetDate).getTime() - now;

      if (distance < 0) {
        clearInterval(timer);
        setTimeLeft({ h: 0, m: 0, s: 0 });
      } else {
        setTimeLeft({
          h: Math.floor((distance / (1000 * 60 * 60))),
          m: Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60)),
          s: Math.floor((distance % (1000 * 60)) / 1000)
        });
      }
    }, 1000);
    return () => clearInterval(timer);
  }, [targetDate]);

  const isExpired = new Date(targetDate).getTime() - new Date().getTime() < 0;

  if (isExpired) return <span className="text-red-500 font-bold text-sm">DEAL EXPIRED</span>;

  return (
    <div className="flex gap-1 items-center font-mono text-sm text-amber-600 bg-amber-50 px-2 py-1 rounded-lg">
      <Clock size={14} />
      <span>{String(timeLeft.h).padStart(2, '0')}:</span>
      <span>{String(timeLeft.m).padStart(2, '0')}:</span>
      <span>{String(timeLeft.s).padStart(2, '0')}</span>
    </div>
  );
};

export default function App() {
  const [user, setUser] = useState(null);
  const [deals, setDeals] = useState([]);
  const [loading, setLoading] = useState(true);
  const [isModalOpen, setIsModalOpen] = useState(false);
  
  // Form State
  const [formData, setFormData] = useState({
    name: '',
    originalPrice: '',
    dealPrice: '',
    stock: '',
    duration: '60' // minutes
  });

  // Auth initialization
  useEffect(() => {
    const initAuth = async () => {
      try {
        if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
          await signInWithCustomToken(auth, __initial_auth_token);
        } else {
          await signInAnonymously(auth);
        }
      } catch (err) {
        console.error("Auth error:", err);
      }
    };
    initAuth();
    const unsubscribe = onAuthStateChanged(auth, (user) => {
      setUser(user);
    });
    return () => unsubscribe();
  }, []);

  // Real-time Data Sync
  useEffect(() => {
    if (!user) return;

    const dealsCollection = collection(db, 'artifacts', appId, 'public', 'data', 'flash_deals');
    const q = query(dealsCollection);

    const unsubscribe = onSnapshot(q, (snapshot) => {
      const dealsList = snapshot.docs.map(doc => ({
        id: doc.id,
        ...doc.data()
      }));
      setDeals(dealsList);
      setLoading(false);
    }, (error) => {
      console.error("Firestore error:", error);
      setLoading(false);
    });

    return () => unsubscribe();
  }, [user]);

  const handleSubmit = async (e) => {
    e.preventDefault();
    if (!user) return;

    const endTime = new Date();
    endTime.setMinutes(endTime.getMinutes() + parseInt(formData.duration));

    try {
      await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'flash_deals'), {
        name: formData.name,
        originalPrice: parseFloat(formData.originalPrice),
        dealPrice: parseFloat(formData.dealPrice),
        stock: parseInt(formData.stock),
        initialStock: parseInt(formData.stock),
        endTime: endTime.toISOString(),
        createdAt: serverTimestamp(),
        createdBy: user.uid
      });
      setIsModalOpen(false);
      setFormData({ name: '', originalPrice: '', dealPrice: '', stock: '', duration: '60' });
    } catch (err) {
      console.error("Error adding deal:", err);
    }
  };

  const updateStock = async (id, currentStock, change) => {
    const newStock = Math.max(0, currentStock + change);
    const docRef = doc(db, 'artifacts', appId, 'public', 'data', 'flash_deals', id);
    await updateDoc(docRef, { stock: newStock });
  };

  const deleteDeal = async (id) => {
    const docRef = doc(db, 'artifacts', appId, 'public', 'data', 'flash_deals', id);
    await deleteDoc(docRef);
  };

  const stats = useMemo(() => {
    const active = deals.filter(d => new Date(d.endTime) > new Date()).length;
    const totalStock = deals.reduce((acc, d) => acc + (d.stock || 0), 0);
    const lowStock = deals.filter(d => d.stock < 5 && d.stock > 0).length;
    return { active, totalStock, lowStock };
  }, [deals]);

  if (loading) {
    return (
      <div className="min-h-screen flex items-center justify-center bg-slate-50">
        <div className="flex flex-col items-center gap-4">
          <Zap className="w-12 h-12 text-blue-600 animate-pulse" />
          <p className="text-slate-500 font-medium">Initializing inventory sync...</p>
        </div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-slate-50 text-slate-900 font-sans pb-20">
      {/* Navigation */}
      <nav className="bg-white border-b border-slate-200 sticky top-0 z-30">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="flex justify-between h-16 items-center">
            <div className="flex items-center gap-2">
              <div className="bg-blue-600 p-2 rounded-lg">
                <Zap className="text-white w-5 h-5" />
              </div>
              <h1 className="text-xl font-bold tracking-tight">Flash<span className="text-blue-600">Stock</span></h1>
            </div>
            <div className="flex items-center gap-4">
              <span className="hidden sm:block text-xs text-slate-400 font-mono">UID: {user?.uid.slice(0,8)}...</span>
              <button 
                onClick={() => setIsModalOpen(true)}
                className="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-xl text-sm font-semibold transition-all flex items-center gap-2 shadow-lg shadow-blue-200"
              >
                <Plus size={18} />
                Create Deal
              </button>
            </div>
          </div>
        </div>
      </nav>

      <main className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 pt-8">
        {/* Stats Grid */}
        <div className="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
          <StatCard title="Active Flash Deals" value={stats.active} icon={Clock} color="bg-amber-500" />
          <StatCard title="Total Inventory" value={stats.totalStock} icon={Box} color="bg-blue-500" />
          <StatCard title="Low Stock Alerts" value={stats.lowStock} icon={AlertCircle} color="bg-rose-500" />
        </div>

        {/* Inventory Section */}
        <div className="flex items-center justify-between mb-6">
          <h2 className="text-lg font-bold flex items-center gap-2">
            <LayoutDashboard className="text-slate-400" size={20} />
            Live Inventory Monitor
          </h2>
          <div className="flex gap-2">
            <span className="bg-emerald-100 text-emerald-700 text-xs px-2 py-1 rounded-full font-bold animate-pulse">● LIVE SYNC</span>
          </div>
        </div>

        {deals.length === 0 ? (
          <div className="bg-white border-2 border-dashed border-slate-200 rounded-3xl p-20 text-center">
            <Package className="w-16 h-16 text-slate-300 mx-auto mb-4" />
            <h3 className="text-lg font-semibold text-slate-600">No active flash deals</h3>
            <p className="text-slate-400 mb-6">Start by creating your first time-limited inventory deal.</p>
            <button onClick={() => setIsModalOpen(true)} className="text-blue-600 font-bold hover:underline">Launch your first deal →</button>
          </div>
        ) : (
          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            {deals.map((deal) => (
              <div key={deal.id} className="bg-white rounded-2xl border border-slate-200 shadow-sm hover:shadow-md transition-shadow overflow-hidden group">
                <div className="p-5">
                  <div className="flex justify-between items-start mb-4">
                    <div>
                      <h3 className="font-bold text-lg text-slate-800 line-clamp-1">{deal.name}</h3>
                      <div className="flex items-center gap-2 mt-1">
                        <span className="text-2xl font-black text-blue-600">${deal.dealPrice}</span>
                        <span className="text-slate-400 line-through text-sm">${deal.originalPrice}</span>
                        <span className="bg-rose-100 text-rose-600 text-[10px] font-bold px-1.5 py-0.5 rounded">
                          -{Math.round((1 - deal.dealPrice / deal.originalPrice) * 100)}%
                        </span>
                      </div>
                    </div>
                    <button 
                      onClick={() => deleteDeal(deal.id)}
                      className="text-slate-300 hover:text-rose-500 p-2 transition-colors"
                    >
                      <Trash2 size={18} />
                    </button>
                  </div>

                  <div className="mb-6">
                    <div className="flex justify-between items-end mb-2">
                      <span className="text-xs font-bold text-slate-500 uppercase tracking-wider">Remaining Stock</span>
                      <span className={`text-sm font-bold ${deal.stock < 5 ? 'text-rose-600' : 'text-slate-700'}`}>
                        {deal.stock} / {deal.initialStock}
                      </span>
                    </div>
                    <div className="h-2 w-full bg-slate-100 rounded-full overflow-hidden">
                      <div 
                        className={`h-full transition-all duration-500 ${deal.stock < 5 ? 'bg-rose-500' : 'bg-blue-500'}`}
                        style={{ width: `${(deal.stock / deal.initialStock) * 100}%` }}
                      ></div>
                    </div>
                  </div>

                  <div className="flex items-center justify-between mt-4 pt-4 border-t border-slate-50">
                    <Countdown targetDate={deal.endTime} />
                    <div className="flex items-center gap-1">
                      <button 
                        onClick={() => updateStock(deal.id, deal.stock, -1)}
                        className="w-8 h-8 flex items-center justify-center rounded-lg border border-slate-200 hover:bg-slate-50 active:scale-95 transition-all text-slate-600 font-bold"
                      >
                        -
                      </button>
                      <button 
                        onClick={() => updateStock(deal.id, deal.stock, 1)}
                        className="w-8 h-8 flex items-center justify-center rounded-lg bg-slate-900 text-white hover:bg-slate-800 active:scale-95 transition-all font-bold"
                      >
                        +
                      </button>
                    </div>
                  </div>
                </div>
                <div className="bg-slate-50 px-5 py-3 flex justify-between items-center group-hover:bg-blue-50 transition-colors">
                  <span className="text-xs text-slate-400 font-medium">SKU: {deal.id.slice(0,6).toUpperCase()}</span>
                  <div className="flex items-center text-blue-600 font-bold text-xs cursor-pointer">
                    View Details <ChevronRight size={14} />
                  </div>
                </div>
              </div>
            ))}
          </div>
        )}
      </main>

      {/* Modal */}
      {isModalOpen && (
        <div className="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-900/40 backdrop-blur-sm">
          <div className="bg-white rounded-3xl w-full max-w-md shadow-2xl overflow-hidden animate-in fade-in zoom-in duration-200">
            <div className="p-6 border-b border-slate-100 flex justify-between items-center">
              <h2 className="text-xl font-bold">New Flash Deal</h2>
              <button onClick={() => setIsModalOpen(false)} className="text-slate-400 hover:text-slate-600">✕</button>
            </div>
            <form onSubmit={handleSubmit} className="p-6 space-y-4">
              <div>
                <label className="block text-sm font-bold text-slate-700 mb-1">Product Name</label>
                <input 
                  type="text" 
                  required
                  placeholder="iPhone 16 Pro Max"
                  className="w-full px-4 py-2 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 focus:outline-none"
                  value={formData.name}
                  onChange={e => setFormData({...formData, name: e.target.value})}
                />
              </div>
              <div className="grid grid-cols-2 gap-4">
                <div>
                  <label className="block text-sm font-bold text-slate-700 mb-1">Original Price ($)</label>
                  <input 
                    type="number" 
                    required
                    placeholder="1200"
                    className="w-full px-4 py-2 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 focus:outline-none"
                    value={formData.originalPrice}
                    onChange={e => setFormData({...formData, originalPrice: e.target.value})}
                  />
                </div>
                <div>
                  <label className="block text-sm font-bold text-slate-700 mb-1">Deal Price ($)</label>
                  <input 
                    type="number" 
                    required
                    placeholder="999"
                    className="w-full px-4 py-2 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 focus:outline-none"
                    value={formData.dealPrice}
                    onChange={e => setFormData({...formData, dealPrice: e.target.value})}
                  />
                </div>
              </div>
              <div className="grid grid-cols-2 gap-4">
                <div>
                  <label className="block text-sm font-bold text-slate-700 mb-1">Stock Amount</label>
                  <input 
                    type="number" 
                    required
                    placeholder="50"
                    className="w-full px-4 py-2 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 focus:outline-none"
                    value={formData.stock}
                    onChange={e => setFormData({...formData, stock: e.target.value})}
                  />
                </div>
                <div>
                  <label className="block text-sm font-bold text-slate-700 mb-1">Duration (Mins)</label>
                  <select 
                    className="w-full px-4 py-2 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 focus:outline-none bg-white"
                    value={formData.duration}
                    onChange={e => setFormData({...formData, duration: e.target.value})}
                  >
                    <option value="5">5 Mins</option>
                    <option value="15">15 Mins</option>
                    <option value="30">30 Mins</option>
                    <option value="60">1 Hour</option>
                    <option value="180">3 Hours</option>
                    <option value="1440">24 Hours</option>
                  </select>
                </div>
              </div>
              <button 
                type="submit"
                className="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 rounded-xl transition-all shadow-lg shadow-blue-200 mt-4 flex items-center justify-center gap-2"
              >
                <Zap size={18} />
                Launch Flash Deal
              </button>
            </form>
          </div>
        </div>
      )}
    </div>
  );
}
