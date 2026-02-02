import React, { useState, useEffect, useMemo } from 'react';
import { 
  Search, 
  MapPin, 
  Globe, 
  Map, 
  Clock, 
  Filter, 
  AlertCircle,
  RefreshCw,
  ExternalLink,
  CheckCircle2,
  XCircle,
  AlertTriangle
} from 'lucide-react';
import { initializeApp } from 'firebase/app';
import { 
  getAuth, 
  signInAnonymously, 
  onAuthStateChanged 
} from 'firebase/auth';
import { 
  getFirestore, 
  doc, 
  onSnapshot, 
  collection, 
  query
} from 'firebase/firestore';

/**
 * PRODUCTION FIREBASE CONFIGURATION
 * Using the explicit keys provided to ensure a live data feed.
 */
const firebaseConfig = {
  apiKey: "AIzaSyBe--2h0tR7n0uxhuqMzzWOhCYdc-o2b4E",
  authDomain: "cvc-openspaces.firebaseapp.com",
  projectId: "cvc-openspaces",
  storageBucket: "cvc-openspaces.firebasestorage.app",
  messagingSenderId: "230322442542",
  appId: "1:230322442542:web:524cc85eaa7af1a5a2aabf",
  measurementId: "G-Z5W4ZCJTPT"
};

// Initialize Firebase services
const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);

/**
 * Helper to determine high contrast text color based on hex background
 */
const getContrastColor = (hexcolor) => {
  if (!hexcolor) return 'text-white';
  const r = parseInt(hexcolor.substring(1, 3), 16);
  const g = parseInt(hexcolor.substring(3, 5), 16);
  const b = parseInt(hexcolor.substring(5, 7), 16);
  const yiq = ((r * 299) + (g * 587) + (b * 114)) / 1000;
  return (yiq >= 128) ? 'text-slate-900' : 'text-white';
};

/**
 * Status Badge Component
 */
const StatusBadge = ({ status, isLink }) => {
  if (!status) return null;
  
  const textColor = getContrastColor(status.color);
  const Icon = status.id === 'open' ? CheckCircle2 : status.id === 'closed' ? XCircle : AlertTriangle;

  const BadgeWrapper = isLink ? 'a' : 'div';
  const wrapperProps = isLink ? { 
    href: status.infoLink, 
    target: "_blank", 
    rel: "noopener noreferrer",
    title: "View Policy/Info"
  } : {};

  return (
    <BadgeWrapper 
      {...wrapperProps}
      className={`inline-flex items-center gap-1.5 px-3 py-1 rounded-full text-[10px] font-bold uppercase tracking-wider transition-all duration-200 shadow-sm border border-black/5 ${textColor} ${isLink ? 'hover:scale-105 active:scale-95 cursor-pointer ring-offset-2 hover:ring-2' : ''}`}
      style={{ 
        backgroundColor: status.color || '#808080',
        borderColor: 'rgba(0,0,0,0.1)'
      }}
    >
      <Icon size={12} strokeWidth={3} />
      {status.text}
      {status.infoLink && <ExternalLink size={10} className="ml-0.5 opacity-70" />}
    </BadgeWrapper>
  );
};

export default function App() {
  const [user, setUser] = useState(null);
  const [facilities, setFacilities] = useState([]);
  const [statuses, setStatuses] = useState({});
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  const [lastUpdated, setLastUpdated] = useState(null);

  // Filter States
  const [searchQuery, setSearchQuery] = useState("");
  const [locationFilter, setLocationFilter] = useState("all");
  const [statusFilter, setStatusFilter] = useState("all");

  /**
   * Initialize Authentication
   */
  useEffect(() => {
    const login = async () => {
      try {
        await signInAnonymously(auth);
      } catch (err) {
        console.error("Auth Error:", err);
        setError("Connection failed. Please check your internet.");
      }
    };
    login();
    return onAuthStateChanged(auth, setUser);
  }, []);

  /**
   * Real-time Data Listeners
   */
  useEffect(() => {
    if (!user) return;

    setLoading(true);
    
    // 1. Fetch Status Configuration
    const statusDocRef = doc(db, "sports_fields_status_config", "statuses");
    const unsubStatus = onSnapshot(statusDocRef, 
      (docSnap) => {
        const data = docSnap.data();
        if (data?.options) {
          const statusMap = data.options.reduce((acc, s) => ({ ...acc, [s.id]: s }), {});
          setStatuses(statusMap);
        }
      },
      (err) => console.error("Status config error:", err)
    );

    // 2. Fetch Fields List
    const fieldsColRef = collection(db, "sports_fields_status");
    const unsubFields = onSnapshot(query(fieldsColRef), 
      (snapshot) => {
        const list = snapshot.docs
          .map(doc => ({ id: doc.id, ...doc.data() }))
          .sort((a, b) => (a.name || "").localeCompare(b.name || ""));
        
        setFacilities(list);
        setLastUpdated(new Date());
        setLoading(false);
      },
      (err) => {
        console.error("Data sync error:", err);
        setError("Failed to load live status feed.");
        setLoading(false);
      }
    );

    return () => {
      unsubStatus();
      unsubFields();
    };
  }, [user]);

  // Derive unique locations for the filter
  const uniqueLocations = useMemo(() => {
    const locs = new Set(facilities.map(f => f.location).filter(Boolean));
    return Array.from(locs).sort();
  }, [facilities]);

  // Apply filters to data
  const filteredFacilities = useMemo(() => {
    return facilities.filter(facility => {
      const q = searchQuery.toLowerCase();
      const matchesSearch = 
        (facility.name || "").toLowerCase().includes(q) || 
        (facility.location || "").toLowerCase().includes(q) ||
        (facility.subFields || []).some(sf => sf.name.toLowerCase().includes(q));
      
      const matchesLocation = locationFilter === "all" || facility.location === locationFilter;
      const matchesStatus = statusFilter === "all" || (facility.subFields || []).some(sf => sf.statusId === statusFilter);

      return matchesSearch && matchesLocation && matchesStatus;
    });
  }, [facilities, searchQuery, locationFilter, statusFilter]);

  if (loading && !facilities.length) {
    return (
      <div className="flex flex-col items-center justify-center min-h-screen bg-slate-50 gap-4">
        <RefreshCw className="animate-spin text-blue-600" size={32} />
        <p className="text-slate-500 font-bold text-sm tracking-wide animate-pulse">CONNECTING TO LIVE FEED...</p>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-slate-50 text-slate-900 pb-12">
      {/* Dynamic Header */}
      <header className="bg-white border-b border-slate-200 sticky top-0 z-20 shadow-sm">
        <div className="max-w-6xl mx-auto px-4 py-4 sm:py-6">
          <div className="flex flex-col lg:flex-row lg:items-center justify-between gap-6">
            <div>
              <h1 className="text-2xl font-black tracking-tight text-slate-800">
                CLARENCE VALLEY <span className="text-blue-600 font-light">FIELD STATUS</span>
              </h1>
              <div className="flex items-center gap-2 mt-1 text-[10px] text-slate-400 font-bold uppercase tracking-widest">
                <div className="w-2 h-2 rounded-full bg-green-500 animate-pulse" />
                Live Sync: {lastUpdated ? lastUpdated.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }) : 'Pending'}
              </div>
            </div>
            
            {/* Filter Section */}
            <div className="flex flex-wrap items-center gap-3">
              <div className="relative flex-grow sm:flex-grow-0 sm:w-64">
                <Search className="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400" size={16} />
                <input 
                  type="text" 
                  placeholder="Search grounds..." 
                  className="w-full pl-10 pr-4 py-2 bg-slate-100 border-none rounded-xl text-sm focus:ring-2 focus:ring-blue-500 transition-all outline-none"
                  value={searchQuery}
                  onChange={(e) => setSearchQuery(e.target.value)}
                />
              </div>
              
              <div className="flex gap-2 w-full sm:w-auto">
                <select 
                  className="flex-1 sm:w-40 px-3 py-2 bg-slate-100 border-none rounded-xl text-xs font-bold focus:ring-2 focus:ring-blue-500 outline-none appearance-none cursor-pointer"
                  value={locationFilter}
                  onChange={(e) => setLocationFilter(e.target.value)}
                >
                  <option value="all text-slate-400">ALL SUBURBS</option>
                  {uniqueLocations.map(loc => <option key={loc} value={loc}>{loc.toUpperCase()}</option>)}
                </select>

                <select 
                  className="flex-1 sm:w-40 px-3 py-2 bg-slate-100 border-none rounded-xl text-xs font-bold focus:ring-2 focus:ring-blue-500 outline-none appearance-none cursor-pointer"
                  value={statusFilter}
                  onChange={(e) => setStatusFilter(e.target.value)}
                >
                  <option value="all">ANY STATUS</option>
                  {Object.values(statuses).map(s => <option key={s.id} value={s.id}>{s.text.toUpperCase()}</option>)}
                </select>
              </div>
            </div>
          </div>
        </div>
      </header>

      {/* Main Grid */}
      <main className="max-w-6xl mx-auto px-4 mt-8">
        {error && (
          <div className="mb-6 p-4 bg-red-50 border border-red-100 text-red-700 rounded-2xl flex items-center gap-3">
            <AlertCircle size={20} />
            <p className="text-sm font-medium">{error}</p>
          </div>
        )}

        {filteredFacilities.length === 0 ? (
          <div className="py-24 text-center bg-white rounded-[2.5rem] border border-slate-100 shadow-sm">
            <div className="bg-slate-50 w-20 h-20 rounded-full flex items-center justify-center mx-auto mb-6">
              <Filter className="text-slate-300" size={40} />
            </div>
            <h3 className="text-xl font-black text-slate-800 uppercase tracking-tight">No results matched</h3>
            <p className="text-slate-500 mt-2 max-w-xs mx-auto text-sm">Adjust your suburb or status filters to try again.</p>
            <button 
              onClick={() => {setSearchQuery(""); setLocationFilter("all"); setStatusFilter("all");}}
              className="mt-8 px-8 py-3 bg-blue-600 text-white rounded-full font-black text-xs uppercase tracking-widest shadow-lg shadow-blue-200 hover:bg-blue-700 transition-all active:scale-95"
            >
              Clear All Filters
            </button>
          </div>
        ) : (
          <div className="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-6">
            {filteredFacilities.map((facility) => (
              <div 
                key={facility.id} 
                className="group flex flex-col bg-white rounded-[2.5rem] shadow-sm hover:shadow-2xl hover:-translate-y-1 transition-all duration-500 border border-slate-100 overflow-hidden"
              >
                {/* Header Information */}
                <div className="p-8 pb-4">
                  <div className="flex justify-between items-start gap-4">
                    <h2 className="text-xl font-black text-slate-800 leading-[1.1] group-hover:text-blue-600 transition-colors">
                      {facility.name}
                    </h2>
                  </div>
                  
                  {facility.location && (
                    <div className="flex items-center gap-1.5 mt-3 text-slate-400">
                      <MapPin size={14} className="flex-shrink-0" />
                      <span className="text-xs font-bold uppercase tracking-wider">{facility.location}</span>
                    </div>
                  )}

                  <div className="flex gap-2 mt-6">
                    {facility.webLink && (
                      <a 
                        href={facility.webLink} 
                        target="_blank" 
                        rel="noopener noreferrer"
                        className="p-2.5 bg-slate-50 rounded-xl text-slate-500 hover:bg-blue-600 hover:text-white transition-all shadow-sm"
                        title="Bookings & Rules"
                      >
                        <Globe size={18} />
                      </a>
                    )}
                    {facility.mapLink && (
                      <a 
                        href={facility.mapLink} 
                        target="_blank" 
                        rel="noopener noreferrer"
                        className="p-2.5 bg-slate-50 rounded-xl text-slate-500 hover:bg-green-600 hover:text-white transition-all shadow-sm"
                        title="Get Directions"
                      >
                        <Map size={18} />
                      </a>
                    )}
                  </div>
                </div>

                {/* Sub-fields Statuses */}
                <div className="mt-auto px-8 pb-8">
                  <div className="space-y-3 pt-6 border-t border-slate-50">
                    {(facility.subFields || []).map((subField) => {
                      const status = statuses[subField.statusId] || { text: 'Unknown', color: '#CBD5E1' };
                      
                      if (statusFilter !== "all" && subField.statusId !== statusFilter) return null;

                      return (
                        <div 
                          key={subField.id} 
                          className="flex items-center justify-between p-4 rounded-[1.5rem] bg-slate-50/50 hover:bg-slate-50 transition-colors"
                        >
                          <span className="text-sm font-black text-slate-700 truncate pr-4 uppercase tracking-tight">
                            {subField.name}
                          </span>
                          <StatusBadge 
                            status={status} 
                            isLink={!!status.infoLink} 
                          />
                        </div>
                      );
                    })}
                  </div>
                </div>
              </div>
            ))}
          </div>
        )}

        <footer className="mt-20 text-center space-y-6">
          <div className="inline-flex items-center gap-3 px-5 py-2.5 bg-blue-50 rounded-full text-blue-700 text-[10px] font-black uppercase tracking-[0.2em] shadow-sm">
            <RefreshCw size={14} className="animate-spin-slow" />
            Live Data Feed Active
          </div>
          <p className="text-slate-400 text-[10px] max-w-lg mx-auto leading-relaxed uppercase font-bold tracking-widest">
            This portal is updated in real-time by Clarence Valley Council field staff. Field closures are mandatory and enforceable. Check signage for final confirmation.
          </p>
        </footer>
      </main>

      <style>{`
        .animate-spin-slow {
          animation: spin 3s linear infinite;
        }
        @keyframes spin {
          from { transform: rotate(0deg); }
          to { transform: rotate(360deg); }
        }
      `}</style>
    </div>
  );
}
