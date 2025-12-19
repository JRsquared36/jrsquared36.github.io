<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pricing Calculator | Resource Management</title>
  <meta name="description" content="Calculate pricing tiers, track contractor hours, and optimize ROI for your business">
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react/18.2.0/umd/react.production.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/18.2.0/umd/react-dom.production.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/7.23.5/babel.min.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    body { font-family: 'Inter', sans-serif; }
  </style>
</head>
<body>
  <div id="root"></div>
  
  <script type="text/babel">
    const { useState, useMemo } = React;

    // Icons as simple SVG components
    const Calculator = ({ className }) => (
      <svg className={className} fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
        <rect x="4" y="2" width="16" height="20" rx="2" />
        <line x1="8" y1="6" x2="16" y2="6" />
        <line x1="8" y1="10" x2="8" y2="10.01" />
        <line x1="12" y1="10" x2="12" y2="10.01" />
        <line x1="16" y1="10" x2="16" y2="10.01" />
        <line x1="8" y1="14" x2="8" y2="14.01" />
        <line x1="12" y1="14" x2="12" y2="14.01" />
        <line x1="16" y1="14" x2="16" y2="14.01" />
        <line x1="8" y1="18" x2="8" y2="18.01" />
        <line x1="12" y1="18" x2="12" y2="18.01" />
        <line x1="16" y1="18" x2="16" y2="18.01" />
      </svg>
    );

    const Users = ({ className }) => (
      <svg className={className} fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
        <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2" />
        <circle cx="9" cy="7" r="4" />
        <path d="M23 21v-2a4 4 0 0 0-3-3.87" />
        <path d="M16 3.13a4 4 0 0 1 0 7.75" />
      </svg>
    );

    const DollarSign = ({ className }) => (
      <svg className={className} fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
        <line x1="12" y1="1" x2="12" y2="23" />
        <path d="M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6" />
      </svg>
    );

    const Clock = ({ className }) => (
      <svg className={className} fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
        <circle cx="12" cy="12" r="10" />
        <polyline points="12 6 12 12 16 14" />
      </svg>
    );

    const TrendingUp = ({ className }) => (
      <svg className={className} fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
        <polyline points="23 6 13.5 15.5 8.5 10.5 1 18" />
        <polyline points="17 6 23 6 23 12" />
      </svg>
    );

    const Briefcase = ({ className }) => (
      <svg className={className} fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
        <rect x="2" y="7" width="20" height="14" rx="2" ry="2" />
        <path d="M16 21V5a2 2 0 0 0-2-2h-4a2 2 0 0 0-2 2v16" />
      </svg>
    );

    const ChevronDown = ({ className }) => (
      <svg className={className} fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
        <polyline points="6 9 12 15 18 9" />
      </svg>
    );

    const ChevronUp = ({ className }) => (
      <svg className={className} fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
        <polyline points="18 15 12 9 6 15" />
      </svg>
    );

    const Plus = ({ className }) => (
      <svg className={className} fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
        <line x1="12" y1="5" x2="12" y2="19" />
        <line x1="5" y1="12" x2="19" y2="12" />
      </svg>
    );

    const Trash2 = ({ className }) => (
      <svg className={className} fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
        <polyline points="3 6 5 6 21 6" />
        <path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2" />
        <line x1="10" y1="11" x2="10" y2="17" />
        <line x1="14" y1="11" x2="14" y2="17" />
      </svg>
    );

    const BarChart3 = ({ className }) => (
      <svg className={className} fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
        <line x1="12" y1="20" x2="12" y2="10" />
        <line x1="18" y1="20" x2="18" y2="4" />
        <line x1="6" y1="20" x2="6" y2="16" />
      </svg>
    );

    const Settings = ({ className }) => (
      <svg className={className} fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
        <circle cx="12" cy="12" r="3" />
        <path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z" />
      </svg>
    );

    const Camera = ({ className }) => (
      <svg className={className} fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
        <path d="M23 19a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h4l2-3h6l2 3h4a2 2 0 0 1 2 2z" />
        <circle cx="12" cy="13" r="4" />
      </svg>
    );

    const Video = ({ className }) => (
      <svg className={className} fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
        <polygon points="23 7 16 12 23 17 23 7" />
        <rect x="1" y="5" width="15" height="14" rx="2" ry="2" />
      </svg>
    );

    const Film = ({ className }) => (
      <svg className={className} fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
        <rect x="2" y="2" width="20" height="20" rx="2.18" ry="2.18" />
        <line x1="7" y1="2" x2="7" y2="22" />
        <line x1="17" y1="2" x2="17" y2="22" />
        <line x1="2" y1="12" x2="22" y2="12" />
        <line x1="2" y1="7" x2="7" y2="7" />
        <line x1="2" y1="17" x2="7" y2="17" />
        <line x1="17" y1="17" x2="22" y2="17" />
        <line x1="17" y1="7" x2="22" y2="7" />
      </svg>
    );

    // Role configurations with default rates
    const DEFAULT_ROLES = [
      { id: 'lead', name: 'Lead Photographer', rate: 75, type: 'internal' },
      { id: 'second', name: 'Second Shooter', rate: 50, type: 'contractor' },
      { id: 'assistant', name: 'Assistant', rate: 25, type: 'contractor' },
      { id: 'editor', name: 'Editor/Retoucher', rate: 40, type: 'contractor' },
      { id: 'coordinator', name: 'Project Coordinator', rate: 35, type: 'internal' },
    ];

    // Default hours (starting point)
    const DEFAULT_HOURS = { lead: 4, second: 0, assistant: 2, editor: 4, coordinator: 1 };

    // Service tier templates
    const SERVICE_TIERS = {
      basic: { name: 'Basic', margin: 1.4, description: 'Entry-level package' },
      standard: { name: 'Standard', margin: 1.6, description: 'Most popular choice' },
      premium: { name: 'Premium', margin: 1.85, description: 'Full-service experience' },
      elite: { name: 'Elite', margin: 2.2, description: 'White-glove service' },
    };

    // Project types
    const PROJECT_TYPES = {
      photo: { name: 'Photo', icon: Camera, color: 'amber' },
      video: { name: 'Video', icon: Video, color: 'blue' },
      photovideo: { name: 'Photo & Video', icon: Film, color: 'purple' },
    };

    function PricingCalculator() {
      const [activeTab, setActiveTab] = useState('calculator');
      const [roles, setRoles] = useState(DEFAULT_ROLES);
      const [selectedProjectType, setSelectedProjectType] = useState('photo');
      const [selectedTier, setSelectedTier] = useState('standard');
      const [showAdvanced, setShowAdvanced] = useState(false);
      
      // Project details
      const [projectName, setProjectName] = useState('');
      const [projectHours, setProjectHours] = useState(DEFAULT_HOURS);
      const [equipmentCost, setEquipmentCost] = useState(100);
      const [travelCost, setTravelCost] = useState(50);
      const [otherCosts, setOtherCosts] = useState(0);
      const [overheadPercent, setOverheadPercent] = useState(15);
      const [customPrice, setCustomPrice] = useState(null);
      
      // Saved projects for comparison
      const [savedProjects, setSavedProjects] = useState([]);
      
      // Calculate costs
      const calculations = useMemo(() => {
        // Labor costs
        const laborCosts = roles.reduce((acc, role) => {
          const hours = projectHours[role.id] || 0;
          return acc + (hours * role.rate);
        }, 0);
        
        // Total hours
        const totalHours = Object.values(projectHours).reduce((a, b) => a + (parseFloat(b) || 0), 0);
        
        // Contractor hours only
        const contractorHours = roles
          .filter(r => r.type === 'contractor')
          .reduce((acc, role) => acc + (parseFloat(projectHours[role.id]) || 0), 0);
        
        // Internal hours
        const internalHours = roles
          .filter(r => r.type === 'internal')
          .reduce((acc, role) => acc + (parseFloat(projectHours[role.id]) || 0), 0);
        
        // Direct costs
        const directCosts = laborCosts + parseFloat(equipmentCost) + parseFloat(travelCost) + parseFloat(otherCosts);
        
        // Overhead
        const overhead = directCosts * (overheadPercent / 100);
        
        // Total costs
        const totalCosts = directCosts + overhead;
        
        // Suggested prices by tier
        const tierPrices = Object.entries(SERVICE_TIERS).reduce((acc, [key, tier]) => {
          acc[key] = Math.ceil(totalCosts * tier.margin / 50) * 50;
          return acc;
        }, {});
        
        // Current tier price
        const suggestedPrice = tierPrices[selectedTier];
        const finalPrice = customPrice !== null ? customPrice : suggestedPrice;
        
        // Profit calculations
        const grossProfit = finalPrice - totalCosts;
        const profitMargin = ((grossProfit / finalPrice) * 100).toFixed(1);
        const roi = ((grossProfit / totalCosts) * 100).toFixed(1);
        
        // Effective hourly rate
        const effectiveHourlyRate = totalHours > 0 ? (grossProfit / totalHours).toFixed(2) : 0;
        
        return {
          laborCosts,
          totalHours,
          contractorHours,
          internalHours,
          directCosts,
          overhead,
          totalCosts,
          tierPrices,
          suggestedPrice,
          finalPrice,
          grossProfit,
          profitMargin,
          roi,
          effectiveHourlyRate,
        };
      }, [roles, projectHours, equipmentCost, travelCost, otherCosts, overheadPercent, selectedTier, customPrice]);
      
      // Handle hour change
      const handleHourChange = (roleId, value) => {
        setProjectHours(prev => ({ ...prev, [roleId]: parseFloat(value) || 0 }));
        setCustomPrice(null);
      };
      
      // Handle role rate change
      const handleRateChange = (roleId, value) => {
        setRoles(prev => prev.map(r => r.id === roleId ? { ...r, rate: parseFloat(value) || 0 } : r));
      };
      
      // Add custom role
      const addRole = () => {
        const newId = `custom_${Date.now()}`;
        setRoles(prev => [...prev, { id: newId, name: 'New Role', rate: 30, type: 'contractor' }]);
        setProjectHours(prev => ({ ...prev, [newId]: 0 }));
      };
      
      // Remove role
      const removeRole = (roleId) => {
        setRoles(prev => prev.filter(r => r.id !== roleId));
        setProjectHours(prev => {
          const newHours = { ...prev };
          delete newHours[roleId];
          return newHours;
        });
      };
      
      // Save project for comparison
      const saveProject = () => {
        const project = {
          id: Date.now(),
          name: projectName || `${PROJECT_TYPES[selectedProjectType].name} - ${SERVICE_TIERS[selectedTier].name}`,
          projectType: selectedProjectType,
          tier: selectedTier,
          hours: { ...projectHours },
          costs: { ...calculations },
          timestamp: new Date().toLocaleDateString(),
        };
        setSavedProjects(prev => [...prev, project]);
        setProjectName('');
      };
      
      // ROI indicator color
      const getRoiColor = (roi) => {
        if (roi >= 60) return 'text-emerald-600';
        if (roi >= 40) return 'text-green-600';
        if (roi >= 20) return 'text-yellow-600';
        return 'text-red-600';
      };
      
      const getMarginColor = (margin) => {
        if (margin >= 40) return 'bg-emerald-500';
        if (margin >= 30) return 'bg-green-500';
        if (margin >= 20) return 'bg-yellow-500';
        return 'bg-red-500';
      };

      const getProjectTypeColors = (type) => {
        switch(type) {
          case 'photo': return { bg: 'bg-amber-500/20', border: 'border-amber-500', text: 'text-amber-400' };
          case 'video': return { bg: 'bg-blue-500/20', border: 'border-blue-500', text: 'text-blue-400' };
          case 'photovideo': return { bg: 'bg-purple-500/20', border: 'border-purple-500', text: 'text-purple-400' };
          default: return { bg: 'bg-amber-500/20', border: 'border-amber-500', text: 'text-amber-400' };
        }
      };

      return (
        <div className="min-h-screen bg-gradient-to-br from-slate-900 via-slate-800 to-slate-900 text-white p-4 md:p-6">
          <div className="max-w-7xl mx-auto">
            {/* Header */}
            <div className="mb-8">
              <div className="flex items-center gap-3 mb-2">
                <div className="p-2 bg-amber-500/20 rounded-lg">
                  <Briefcase className="w-8 h-8 text-amber-400" />
                </div>
                <h1 className="text-2xl md:text-3xl font-bold bg-gradient-to-r from-amber-400 to-orange-400 bg-clip-text text-transparent">
                  Pricing Calculator
                </h1>
              </div>
              <p className="text-slate-400">Price optimization & resource management for scaling your business</p>
            </div>
            
            {/* Tab Navigation */}
            <div className="flex gap-2 mb-6 bg-slate-800/50 p-1 rounded-xl w-fit">
              {[
                { id: 'calculator', label: 'Price Calculator', icon: Calculator },
                { id: 'resources', label: 'Resources', icon: Users },
                { id: 'analytics', label: 'Analytics', icon: BarChart3 },
              ].map(tab => (
                <button
                  key={tab.id}
                  onClick={() => setActiveTab(tab.id)}
                  className={`flex items-center gap-2 px-4 py-2 rounded-lg transition-all ${
                    activeTab === tab.id 
                      ? 'bg-amber-500 text-slate-900 font-semibold' 
                      : 'text-slate-400 hover:text-white hover:bg-slate-700/50'
                  }`}
                >
                  <tab.icon className="w-4 h-4" />
                  <span className="hidden sm:inline">{tab.label}</span>
                </button>
              ))}
            </div>

            {/* Calculator Tab */}
            {activeTab === 'calculator' && (
              <div className="grid lg:grid-cols-3 gap-6">
                {/* Left Column - Input */}
                <div className="lg:col-span-2 space-y-6">
                  {/* Project Setup */}
                  <div className="bg-slate-800/50 backdrop-blur rounded-2xl p-6 border border-slate-700/50">
                    <h2 className="text-lg font-semibold mb-4 flex items-center gap-2">
                      <Settings className="w-5 h-5 text-amber-400" />
                      Project Setup
                    </h2>
                    
                    <div className="mb-4">
                      <label className="block text-sm text-slate-400 mb-1">Project Name (Optional)</label>
                      <input
                        type="text"
                        value={projectName}
                        onChange={(e) => setProjectName(e.target.value)}
                        placeholder="e.g., Smith Wedding, Corporate Event"
                        className="w-full bg-slate-700/50 border border-slate-600 rounded-lg px-4 py-2 focus:outline-none focus:border-amber-500"
                      />
                    </div>

                    {/* Project Type Selection */}
                    <div className="mb-4">
                      <label className="block text-sm text-slate-400 mb-2">Project Type</label>
                      <div className="grid grid-cols-3 gap-3">
                        {Object.entries(PROJECT_TYPES).map(([key, type]) => {
                          const colors = getProjectTypeColors(key);
                          const IconComponent = type.icon;
                          return (
                            <button
                              key={key}
                              onClick={() => setSelectedProjectType(key)}
                              className={`p-4 rounded-xl border-2 transition-all flex flex-col items-center gap-2 ${
                                selectedProjectType === key
                                  ? `${colors.bg} ${colors.border} ${colors.text}`
                                  : 'bg-slate-700/30 border-slate-600 hover:border-slate-500 text-slate-300'
                              }`}
                            >
                              <IconComponent className="w-6 h-6" />
                              <div className="font-semibold text-sm">{type.name}</div>
                            </button>
                          );
                        })}
                      </div>
                    </div>
                    
                    {/* Service Tier Selection */}
                    <div className="mb-4">
                      <label className="block text-sm text-slate-400 mb-2">Service Tier</label>
                      <div className="grid grid-cols-2 md:grid-cols-4 gap-2">
                        {Object.entries(SERVICE_TIERS).map(([key, tier]) => (
                          <button
                            key={key}
                            onClick={() => { setSelectedTier(key); setCustomPrice(null); }}
                            className={`p-3 rounded-xl border transition-all ${
                              selectedTier === key
                                ? 'bg-amber-500/20 border-amber-500 text-amber-400'
                                : 'bg-slate-700/30 border-slate-600 hover:border-slate-500'
                            }`}
                          >
                            <div className="font-semibold">{tier.name}</div>
                            <div className="text-xs text-slate-400">{tier.margin}x margin</div>
                          </button>
                        ))}
                      </div>
                    </div>
                  </div>
                  
                  {/* Resource Allocation */}
                  <div className="bg-slate-800/50 backdrop-blur rounded-2xl p-6 border border-slate-700/50">
                    <div className="flex items-center justify-between mb-4">
                      <h2 className="text-lg font-semibold flex items-center gap-2">
                        <Clock className="w-5 h-5 text-amber-400" />
                        Resource Allocation (Hours)
                      </h2>
                      <button
                        onClick={addRole}
                        className="flex items-center gap-1 text-sm text-amber-400 hover:text-amber-300"
                      >
                        <Plus className="w-4 h-4" /> Add Role
                      </button>
                    </div>
                    
                    <div className="space-y-3">
                      {roles.map(role => (
                        <div key={role.id} className="flex items-center gap-3 bg-slate-700/30 rounded-xl p-3">
                          <div className="flex-1 min-w-0">
                            <input
                              type="text"
                              value={role.name}
                              onChange={(e) => setRoles(prev => prev.map(r => r.id === role.id ? { ...r, name: e.target.value } : r))}
                              className="bg-transparent font-medium w-full focus:outline-none"
                            />
                            <div className="flex items-center gap-2 mt-1">
                              <span className={`text-xs px-2 py-0.5 rounded ${
                                role.type === 'contractor' ? 'bg-blue-500/20 text-blue-400' : 'bg-green-500/20 text-green-400'
                              }`}>
                                {role.type}
                              </span>
                              <span className="text-xs text-slate-400">${role.rate}/hr</span>
                            </div>
                          </div>
                          <div className="flex items-center gap-2">
                            <div className="w-20">
                              <input
                                type="number"
                                value={projectHours[role.id] || 0}
                                onChange={(e) => handleHourChange(role.id, e.target.value)}
                                min="0"
                                step="0.5"
                                className="w-full bg-slate-600/50 border border-slate-500 rounded-lg px-3 py-1.5 text-center focus:outline-none focus:border-amber-500"
                              />
                            </div>
                            <span className="text-slate-400 text-sm w-8">hrs</span>
                            {!DEFAULT_ROLES.find(r => r.id === role.id) && (
                              <button
                                onClick={() => removeRole(role.id)}
                                className="text-red-400 hover:text-red-300 p-1"
                              >
                                <Trash2 className="w-4 h-4" />
                              </button>
                            )}
                          </div>
                        </div>
                      ))}
                    </div>
                  </div>
                  
                  {/* Additional Costs */}
                  <div className="bg-slate-800/50 backdrop-blur rounded-2xl p-6 border border-slate-700/50">
                    <button
                      onClick={() => setShowAdvanced(!showAdvanced)}
                      className="w-full flex items-center justify-between text-lg font-semibold mb-4"
                    >
                      <span className="flex items-center gap-2">
                        <DollarSign className="w-5 h-5 text-amber-400" />
                        Additional Costs & Settings
                      </span>
                      {showAdvanced ? <ChevronUp className="w-5 h-5" /> : <ChevronDown className="w-5 h-5" />}
                    </button>
                    
                    {showAdvanced && (
                      <div className="grid sm:grid-cols-2 gap-4">
                        <div>
                          <label className="block text-sm text-slate-400 mb-1">Equipment Rental/Wear</label>
                          <div className="relative">
                            <span className="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400">$</span>
                            <input
                              type="number"
                              value={equipmentCost}
                              onChange={(e) => setEquipmentCost(e.target.value)}
                              className="w-full bg-slate-700/50 border border-slate-600 rounded-lg pl-8 pr-4 py-2 focus:outline-none focus:border-amber-500"
                            />
                          </div>
                        </div>
                        <div>
                          <label className="block text-sm text-slate-400 mb-1">Travel Expenses</label>
                          <div className="relative">
                            <span className="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400">$</span>
                            <input
                              type="number"
                              value={travelCost}
                              onChange={(e) => setTravelCost(e.target.value)}
                              className="w-full bg-slate-700/50 border border-slate-600 rounded-lg pl-8 pr-4 py-2 focus:outline-none focus:border-amber-500"
                            />
                          </div>
                        </div>
                        <div>
                          <label className="block text-sm text-slate-400 mb-1">Other Costs</label>
                          <div className="relative">
                            <span className="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400">$</span>
                            <input
                              type="number"
                              value={otherCosts}
                              onChange={(e) => setOtherCosts(e.target.value)}
                              className="w-full bg-slate-700/50 border border-slate-600 rounded-lg pl-8 pr-4 py-2 focus:outline-none focus:border-amber-500"
                            />
                          </div>
                        </div>
                        <div>
                          <label className="block text-sm text-slate-400 mb-1">Overhead %</label>
                          <div className="relative">
                            <input
                              type="number"
                              value={overheadPercent}
                              onChange={(e) => setOverheadPercent(e.target.value)}
                              className="w-full bg-slate-700/50 border border-slate-600 rounded-lg px-4 py-2 pr-8 focus:outline-none focus:border-amber-500"
                            />
                            <span className="absolute right-3 top-1/2 -translate-y-1/2 text-slate-400">%</span>
                          </div>
                        </div>
                      </div>
                    )}
                  </div>
                </div>
                
                {/* Right Column - Results */}
                <div className="space-y-6">
                  {/* Price Card */}
                  <div className="bg-gradient-to-br from-amber-500/20 to-orange-500/20 backdrop-blur rounded-2xl p-6 border border-amber-500/30">
                    <h2 className="text-lg font-semibold mb-4 flex items-center gap-2">
                      <TrendingUp className="w-5 h-5 text-amber-400" />
                      Pricing Summary
                    </h2>
                    
                    <div className="text-center mb-6">
                      <div className="text-sm text-slate-400 mb-1">Suggested Price ({SERVICE_TIERS[selectedTier].name})</div>
                      <div className="text-4xl font-bold text-amber-400">
                        ${calculations.suggestedPrice.toLocaleString()}
                      </div>
                    </div>
                    
                    <div className="mb-4">
                      <label className="block text-sm text-slate-400 mb-1">Custom Price Override</label>
                      <div className="relative">
                        <span className="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400">$</span>
                        <input
                          type="number"
                          value={customPrice ?? ''}
                          onChange={(e) => setCustomPrice(e.target.value ? parseFloat(e.target.value) : null)}
                          placeholder={calculations.suggestedPrice.toString()}
                          className="w-full bg-slate-700/50 border border-slate-600 rounded-lg pl-8 pr-4 py-2 focus:outline-none focus:border-amber-500"
                        />
                      </div>
                    </div>
                    
                    {/* Tier Comparison */}
                    <div className="space-y-2 mb-4">
                      <div className="text-sm text-slate-400">All Tier Prices:</div>
                      {Object.entries(calculations.tierPrices).map(([key, price]) => (
                        <div 
                          key={key}
                          className={`flex justify-between text-sm p-2 rounded-lg ${
                            key === selectedTier ? 'bg-amber-500/20' : 'bg-slate-700/30'
                          }`}
                        >
                          <span>{SERVICE_TIERS[key].name}</span>
                          <span className="font-semibold">${price.toLocaleString()}</span>
                        </div>
                      ))}
                    </div>
                    
                    <button
                      onClick={saveProject}
                      className="w-full py-2 bg-amber-500 hover:bg-amber-400 text-slate-900 font-semibold rounded-lg transition-colors"
                    >
                      Save for Comparison
                    </button>
                  </div>
                  
                  {/* ROI Card */}
                  <div className="bg-slate-800/50 backdrop-blur rounded-2xl p-6 border border-slate-700/50">
                    <h2 className="text-lg font-semibold mb-4">Profitability Analysis</h2>
                    
                    <div className="space-y-4">
                      <div className="flex justify-between items-center">
                        <span className="text-slate-400">Total Costs</span>
                        <span className="font-semibold">${calculations.totalCosts.toFixed(2)}</span>
                      </div>
                      <div className="flex justify-between items-center">
                        <span className="text-slate-400">Final Price</span>
                        <span className="font-semibold">${calculations.finalPrice.toLocaleString()}</span>
                      </div>
                      <div className="h-px bg-slate-700"></div>
                      <div className="flex justify-between items-center">
                        <span className="text-slate-400">Gross Profit</span>
                        <span className={`font-semibold ${calculations.grossProfit >= 0 ? 'text-green-400' : 'text-red-400'}`}>
                          ${calculations.grossProfit.toFixed(2)}
                        </span>
                      </div>
                      
                      {/* Profit Margin Bar */}
                      <div>
                        <div className="flex justify-between text-sm mb-1">
                          <span className="text-slate-400">Profit Margin</span>
                          <span className="font-semibold">{calculations.profitMargin}%</span>
                        </div>
                        <div className="h-2 bg-slate-700 rounded-full overflow-hidden">
                          <div 
                            className={`h-full ${getMarginColor(calculations.profitMargin)} transition-all`}
                            style={{ width: `${Math.min(Math.max(calculations.profitMargin, 0), 100)}%` }}
                          ></div>
                        </div>
                      </div>
                      
                      <div className="flex justify-between items-center">
                        <span className="text-slate-400">ROI</span>
                        <span className={`font-bold text-xl ${getRoiColor(calculations.roi)}`}>
                          {calculations.roi}%
                        </span>
                      </div>
                      
                      <div className="flex justify-between items-center">
                        <span className="text-slate-400">Effective $/hr (profit)</span>
                        <span className="font-semibold">${calculations.effectiveHourlyRate}/hr</span>
                      </div>
                    </div>
                  </div>
                  
                  {/* Hours Summary */}
                  <div className="bg-slate-800/50 backdrop-blur rounded-2xl p-6 border border-slate-700/50">
                    <h2 className="text-lg font-semibold mb-4">Hours Summary</h2>
                    <div className="grid grid-cols-3 gap-4 text-center">
                      <div className="bg-slate-700/30 rounded-xl p-3">
                        <div className="text-2xl font-bold text-amber-400">{calculations.totalHours}</div>
                        <div className="text-xs text-slate-400">Total Hours</div>
                      </div>
                      <div className="bg-slate-700/30 rounded-xl p-3">
                        <div className="text-2xl font-bold text-blue-400">{calculations.contractorHours}</div>
                        <div className="text-xs text-slate-400">Contractor</div>
                      </div>
                      <div className="bg-slate-700/30 rounded-xl p-3">
                        <div className="text-2xl font-bold text-green-400">{calculations.internalHours}</div>
                        <div className="text-xs text-slate-400">Internal</div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            )}

            {/* Resources Tab */}
            {activeTab === 'resources' && (
              <div className="grid lg:grid-cols-2 gap-6">
                <div className="bg-slate-800/50 backdrop-blur rounded-2xl p-6 border border-slate-700/50">
                  <h2 className="text-lg font-semibold mb-4 flex items-center gap-2">
                    <Users className="w-5 h-5 text-amber-400" />
                    Team Rate Management
                  </h2>
                  <p className="text-sm text-slate-400 mb-4">Configure hourly rates for all team members and contractors</p>
                  
                  <div className="space-y-3">
                    {roles.map(role => (
                      <div key={role.id} className="bg-slate-700/30 rounded-xl p-4">
                        <div className="flex items-center justify-between mb-2">
                          <input
                            type="text"
                            value={role.name}
                            onChange={(e) => setRoles(prev => prev.map(r => r.id === role.id ? { ...r, name: e.target.value } : r))}
                            className="bg-transparent font-medium text-lg focus:outline-none"
                          />
                          <select
                            value={role.type}
                            onChange={(e) => setRoles(prev => prev.map(r => r.id === role.id ? { ...r, type: e.target.value } : r))}
                            className="bg-slate-600/50 border border-slate-500 rounded-lg px-3 py-1 text-sm focus:outline-none"
                          >
                            <option value="internal">Internal</option>
                            <option value="contractor">Contractor</option>
                          </select>
                        </div>
                        <div className="flex items-center gap-3">
                          <label className="text-sm text-slate-400">Hourly Rate:</label>
                          <div className="relative flex-1">
                            <span className="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400">$</span>
                            <input
                              type="number"
                              value={role.rate}
                              onChange={(e) => handleRateChange(role.id, e.target.value)}
                              className="w-full bg-slate-600/50 border border-slate-500 rounded-lg pl-8 pr-4 py-2 focus:outline-none focus:border-amber-500"
                            />
                          </div>
                          {!DEFAULT_ROLES.find(r => r.id === role.id) && (
                            <button
                              onClick={() => removeRole(role.id)}
                              className="text-red-400 hover:text-red-300 p-2"
                            >
                              <Trash2 className="w-5 h-5" />
                            </button>
                          )}
                        </div>
                      </div>
                    ))}
                  </div>
                  
                  <button
                    onClick={addRole}
                    className="w-full mt-4 py-3 border-2 border-dashed border-slate-600 hover:border-amber-500 rounded-xl text-slate-400 hover:text-amber-400 flex items-center justify-center gap-2 transition-colors"
                  >
                    <Plus className="w-5 h-5" /> Add New Role
                  </button>
                </div>
                
                <div className="bg-slate-800/50 backdrop-blur rounded-2xl p-6 border border-slate-700/50">
                  <h2 className="text-lg font-semibold mb-4">Cost Breakdown by Role Type</h2>
                  
                  <div className="space-y-6">
                    <div>
                      <div className="flex justify-between mb-2">
                        <span className="text-slate-400">Contractor Costs</span>
                        <span className="font-semibold text-blue-400">
                          ${roles.filter(r => r.type === 'contractor').reduce((acc, r) => acc + ((projectHours[r.id] || 0) * r.rate), 0).toFixed(2)}
                        </span>
                      </div>
                      <div className="space-y-1">
                        {roles.filter(r => r.type === 'contractor').map(role => (
                          <div key={role.id} className="flex justify-between text-sm text-slate-400 pl-4">
                            <span>{role.name}: {projectHours[role.id] || 0} hrs × ${role.rate}</span>
                            <span>${((projectHours[role.id] || 0) * role.rate).toFixed(2)}</span>
                          </div>
                        ))}
                      </div>
                    </div>
                    
                    <div>
                      <div className="flex justify-between mb-2">
                        <span className="text-slate-400">Internal Costs</span>
                        <span className="font-semibold text-green-400">
                          ${roles.filter(r => r.type === 'internal').reduce((acc, r) => acc + ((projectHours[r.id] || 0) * r.rate), 0).toFixed(2)}
                        </span>
                      </div>
                      <div className="space-y-1">
                        {roles.filter(r => r.type === 'internal').map(role => (
                          <div key={role.id} className="flex justify-between text-sm text-slate-400 pl-4">
                            <span>{role.name}: {projectHours[role.id] || 0} hrs × ${role.rate}</span>
                            <span>${((projectHours[role.id] || 0) * role.rate).toFixed(2)}</span>
                          </div>
                        ))}
                      </div>
                    </div>
                    
                    <div className="h-px bg-slate-700"></div>
                    
                    <div className="flex justify-between">
                      <span className="font-semibold">Total Labor</span>
                      <span className="font-bold text-amber-400">${calculations.laborCosts.toFixed(2)}</span>
                    </div>
                  </div>
                </div>
              </div>
            )}

            {/* Analytics Tab */}
            {activeTab === 'analytics' && (
              <div className="space-y-6">
                {savedProjects.length === 0 ? (
                  <div className="bg-slate-800/50 backdrop-blur rounded-2xl p-12 border border-slate-700/50 text-center">
                    <BarChart3 className="w-16 h-16 text-slate-600 mx-auto mb-4" />
                    <h3 className="text-xl font-semibold mb-2">No Saved Projects Yet</h3>
                    <p className="text-slate-400 mb-6">Save projects from the calculator to compare them here</p>
                    <button
                      onClick={() => setActiveTab('calculator')}
                      className="px-6 py-2 bg-amber-500 hover:bg-amber-400 text-slate-900 font-semibold rounded-lg transition-colors"
                    >
                      Go to Calculator
                    </button>
                  </div>
                ) : (
                  <>
                    <div className="bg-slate-800/50 backdrop-blur rounded-2xl p-6 border border-slate-700/50">
                      <h2 className="text-lg font-semibold mb-4">Saved Projects Comparison</h2>
                      <div className="overflow-x-auto">
                        <table className="w-full">
                          <thead>
                            <tr className="text-left border-b border-slate-700">
                              <th className="pb-3 text-slate-400 font-medium">Project</th>
                              <th className="pb-3 text-slate-400 font-medium">Type</th>
                              <th className="pb-3 text-slate-400 font-medium">Tier</th>
                              <th className="pb-3 text-slate-400 font-medium text-right">Price</th>
                              <th className="pb-3 text-slate-400 font-medium text-right">Cost</th>
                              <th className="pb-3 text-slate-400 font-medium text-right">Profit</th>
                              <th className="pb-3 text-slate-400 font-medium text-right">ROI</th>
                              <th className="pb-3 text-slate-400 font-medium text-right">Hours</th>
                              <th className="pb-3"></th>
                            </tr>
                          </thead>
                          <tbody>
                            {savedProjects.map(project => {
                              const colors = getProjectTypeColors(project.projectType);
                              return (
                                <tr key={project.id} className="border-b border-slate-700/50">
                                  <td className="py-3 font-medium">{project.name}</td>
                                  <td className="py-3">
                                    <span className={`px-2 py-1 ${colors.bg} ${colors.text} rounded text-sm`}>
                                      {PROJECT_TYPES[project.projectType]?.name}
                                    </span>
                                  </td>
                                  <td className="py-3">
                                    <span className="px-2 py-1 bg-amber-500/20 text-amber-400 rounded text-sm">
                                      {SERVICE_TIERS[project.tier]?.name}
                                    </span>
                                  </td>
                                  <td className="py-3 text-right font-semibold">${project.costs.finalPrice.toLocaleString()}</td>
                                  <td className="py-3 text-right text-slate-400">${project.costs.totalCosts.toFixed(0)}</td>
                                  <td className={`py-3 text-right font-semibold ${project.costs.grossProfit >= 0 ? 'text-green-400' : 'text-red-400'}`}>
                                    ${project.costs.grossProfit.toFixed(0)}
                                  </td>
                                  <td className={`py-3 text-right font-bold ${getRoiColor(project.costs.roi)}`}>
                                    {project.costs.roi}%
                                  </td>
                                  <td className="py-3 text-right text-slate-400">{project.costs.totalHours}</td>
                                  <td className="py-3 text-right">
                                    <button
                                      onClick={() => setSavedProjects(prev => prev.filter(p => p.id !== project.id))}
                                      className="text-red-400 hover:text-red-300 p-1"
                                    >
                                      <Trash2 className="w-4 h-4" />
                                    </button>
                                  </td>
                                </tr>
                              );
                            })}
                          </tbody>
                        </table>
                      </div>
                    </div>
                    
                    {/* Summary Stats */}
                    <div className="grid sm:grid-cols-2 lg:grid-cols-4 gap-4">
                      <div className="bg-slate-800/50 backdrop-blur rounded-xl p-4 border border-slate-700/50">
                        <div className="text-sm text-slate-400 mb-1">Total Revenue (Saved)</div>
                        <div className="text-2xl font-bold text-amber-400">
                          ${savedProjects.reduce((acc, p) => acc + p.costs.finalPrice, 0).toLocaleString()}
                        </div>
                      </div>
                      <div className="bg-slate-800/50 backdrop-blur rounded-xl p-4 border border-slate-700/50">
                        <div className="text-sm text-slate-400 mb-1">Total Profit</div>
                        <div className="text-2xl font-bold text-green-400">
                          ${savedProjects.reduce((acc, p) => acc + p.costs.grossProfit, 0).toFixed(0)}
                        </div>
                      </div>
                      <div className="bg-slate-800/50 backdrop-blur rounded-xl p-4 border border-slate-700/50">
                        <div className="text-sm text-slate-400 mb-1">Avg ROI</div>
                        <div className="text-2xl font-bold text-emerald-400">
                          {(savedProjects.reduce((acc, p) => acc + parseFloat(p.costs.roi), 0) / savedProjects.length).toFixed(1)}%
                        </div>
                      </div>
                      <div className="bg-slate-800/50 backdrop-blur rounded-xl p-4 border border-slate-700/50">
                        <div className="text-sm text-slate-400 mb-1">Total Hours</div>
                        <div className="text-2xl font-bold text-blue-400">
                          {savedProjects.reduce((acc, p) => acc + p.costs.totalHours, 0)}
                        </div>
                      </div>
                    </div>
                  </>
                )}
              </div>
            )}
          </div>
        </div>
      );
    }

    ReactDOM.render(<PricingCalculator />, document.getElementById('root'));
  </script>
</body>
</html>
