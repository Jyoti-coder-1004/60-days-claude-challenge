<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Operation Lifeline: Supply Chain Crisis Lab</title>
    <!-- React, Babel, and Google Fonts CDNs -->
    <script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-main: #0b0f19;
            --bg-card: #151d30;
            --bg-card-hover: #1e2942;
            --border-color: #24324f;
            --text-primary: #f3f4f6;
            --text-secondary: #9ca3af;
            --accent-blue: #3b82f6;
            --accent-green: #10b981;
            --accent-red: #ef4444;
            --accent-yellow: #f59e0b;
            --accent-purple: #8b5cf6;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Plus Jakarta Sans', sans-serif;
        }

        body {
            background-color: var(--bg-main);
            color: var(--text-primary);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            overflow-x: hidden;
        }

        /* Utility Classes & Animations */
        .fade-in {
            animation: fadeIn 0.5s ease-out forwards;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: var(--bg-main);
        }
        ::-webkit-scrollbar-thumb {
            background: var(--border-color);
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: var(--text-secondary);
        }
    </style>
</head>
<body>
    <div id="root"></div>

    <script type="text/babel">
        const { useState, useEffect } = React;

        // --- DATA POOLS ---
        const INDUSTRIES = [
            { name: "Automotive Electronics", desc: "Just-In-Time manufacturing with razor-thin component buffers." },
            { name: "Biomedical Devices", desc: "Highly regulated temperature-controlled supply lines requiring ultra-pure raw inputs." },
            { name: "Renewable Energy Grid Tech", desc: "Global source network reliant heavily on specific rare earth mineral refine centers." },
            { name: "High-Performance Aerospace", desc: "Extremely long structural component lead times and rigorous validation phases." }
        ];

        const CRISES = [
            {
                type: "Factory Fire",
                title: "Tier-1 Microprocessor Foundry Fire",
                desc: "A catastrophic blaze has taken out your main micro-controller production line in East Asia, instantly cutting off 45% of your core sub-assembly incoming volume.",
                urgency: "CRITICAL",
                impact: "Severe operational stall within 14 days as current safety stock depletes.",
                metricsEffect: { cost: 15, inventory: -30, profit: -25, speed: -35, satisfaction: -20 }
            },
            {
                type: "Supplier Bankruptcy",
                title: "Primary Raw Material Refiner Liquidation",
                desc: "Your sole-source provider for processed alloy materials abruptly declared bankruptcy and locked their facility gates due to unmanageable debt structural collapses.",
                urgency: "HIGH",
                impact: "Complete immediate freeze on critical raw inputs with zero notice.",
                metricsEffect: { cost: 20, inventory: -25, profit: -20, speed: -20, satisfaction: -15 }
            },
            {
                type: "Port Strike",
                title: "Massive Trans-Pacific Gateway Labor Strike",
                desc: "A failure in collective bargaining has completely halted freight handling at your primary maritime import hub, stranding dozens of your ocean containers offshore.",
                urgency: "HIGH",
                impact: "Intermittent logistics blind-spots and soaring storage demurrage charges.",
                metricsEffect: { cost: 25, inventory: 15, profit: -15, speed: -45, satisfaction: -25 }
            },
            {
                type: "Cyberattack",
                title: "Ransomware Breach on Central ERP Logistics Network",
                desc: "An aggressive ransomware group has successfully encrypted your global warehouse management and dispatch systems, leaving operations blind to exact inventory locations.",
                urgency: "CRITICAL",
                impact: "Total shipping blind spots, multi-facility gridlocks, and heavy administrative overhead.",
                metricsEffect: { cost: 20, inventory: 5, profit: -30, speed: -40, satisfaction: -30 }
            }
        ];

        const WAR_ROOM_OPTIONS = [
            { id: 1, label: "Authorize Premium Air Freight Expediting", cost: 25, inventory: -10, speed: 35, satisfaction: 20, profit: -15, why: "Bypasses standard blocked maritime corridors entirely by switching to immediate air lifting, dramatically cutting transport delay cycles but adding vast spot-market transit premiums." },
            { id: 2, label: "Activate High-Cost Secondary Source Backups", cost: 30, inventory: 20, speed: 5, satisfaction: 10, profit: -20, why: "Brings unvetted secondary backup vendors online instantly. This solves volume shortfalls but incurs heavy initial onboarding, tooling calibration, and premium raw component unit prices." },
            { id: 3, label: "Implement Product Mix Simplification (De-scoping)", cost: -10, inventory: 15, speed: 20, satisfaction: -15, profit: 10, why: "Temporarily freezes secondary customization options to concentrate all available components exclusively on a singular high-volume core standard build, increasing assembly throughout while reducing buyer choice." },
            { id: 4, label: "Institute Proactive Rationing and Allocation", cost: 5, inventory: 10, speed: -5, satisfaction: -30, profit: -5, why: "Strictly caps order fulfillment volumes for all active clients based on historical baseline averages to avoid a complete stockout, protecting small-tier buyers but severely frustrating VIP enterprise clients." },
            { id: 5, label: "Absorb Delays and Extend Estimated Lead Times", cost: -15, inventory: -5, speed: -30, satisfaction: -25, profit: 5, why: "Chooses to wait out the current peak disruption window without paying spot-market premiums. Conserves capital reserves but stretches customer delivery windows out by months." },
            { id: 6, label: "Deploy Specialized Engineering Taskforce for Sourcing Alterations", cost: 15, inventory: -5, speed: 10, satisfaction: 5, profit: -10, why: "Dispatches senior product specialists to alter specifications to accept readily available alternative parts, resolving blockages via design flexibility though requiring engineering cycle hours." }
        ];

        const NEGOTIATION_ROUNDS = [
            {
                round: 1,
                prompt: "The supplier demands a non-negotiable 25% immediate unit price premium to prioritize your emergency purchase order over their other contract clients. How do you respond?",
                choices: [
                    { text: "Accept fully to guarantee priority access.", trust: 20, price: 25, leadTime: -15 },
                    { text: "Counter-offer a 10% premium linked to rigid on-time delivery KPIs.", trust: 10, price: 10, leadTime: -5 },
                    { text: "Reject firmly, citing long-term partner contract terms.", trust: -15, price: 0, leadTime: 15 },
                    { text: "Propose a multi-year exclusive volume commitment in exchange for keeping current pricing fixed.", trust: 15, price: 5, leadTime: -10 }
                ]
            },
            {
                round: 2,
                prompt: "To hit the accelerated delivery timeline, the supplier wants to skip standard pre-shipment quality verification protocols. What is your stance?",
                choices: [
                    { text: "Agree to waive testing to shave 10 days off lead time.", trust: 10, price: -5, leadTime: -20 },
                    { text: "Insist on statistical batch sampling instead of full checks as a middle ground.", trust: 5, price: 0, leadTime: -5 },
                    { text: "Refuse to skip quality protocols; any defect creates catastrophic line shutdowns later.", trust: -5, price: 5, leadTime: 15 },
                    { text: "Offer to pay an extra shift fee for supplier quality staff to work overtime and maintain checks.", trust: 15, price: 15, leadTime: -10 }
                ]
            },
            {
                round: 3,
                prompt: "The supplier reports logistics capacity shortfalls and asks you to assume full Ex-Works (EXW) structural liability and pick up items directly at their factory gate.",
                choices: [
                    { text: "Take on complete logistics responsibility and arrange immediate carrier pickups.", trust: 15, price: 10, leadTime: -10 },
                    { text: "Propose a Shared Risk framework (FCA) to split freight management and liabilities.", trust: 10, price: 5, leadTime: -5 },
                    { text: "Insist on traditional Delivered Duty Paid (DDP) terms to keep liability on them.", trust: -10, price: 0, leadTime: 10 },
                    { text: "Threaten legal review for non-performance under contract guidelines.", trust: -25, price: -5, leadTime: 15 }
                ]
            },
            {
                round: 4,
                prompt: "As final contracts draft, the supplier requests a substantial 50% upfront cash advance payment to secure raw material allocations.",
                choices: [
                    { text: "Wire the 50% advance immediately to prevent any scheduling delays.", trust: 20, price: 5, leadTime: -5 },
                    { text: "Counter with a milestone-based structure: 25% now, 25% upon verified port departure.", trust: 10, price: 0, leadTime: 0 },
                    { text: "Offer an irrevocable Letter of Credit instead to safeguard corporate working capital.", trust: 5, price: 5, leadTime: 5 },
                    { text: "Refuse outright and demand standard Net 60 payment terms.", trust: -20, price: -10, leadTime: 20 }
                ]
            }
        ];

        const BOARDROOM_QUESTIONS = [
            {
                q: "During an active earnings call, an analyst asks about the specific operational impact of the ongoing supply chain disruption. What is your communication strategy?",
                options: [
                    { text: "Provide transparent, quantified impact metrics alongside an active multi-tiered remediation strategy.", score: 20, type: "Resilience" },
                    { text: "Downplay the disruption's impact to preserve share prices, claiming full recovery within days.", score: 0, type: "Risk Management" },
                    { text: "Deflect the question by pointing out that industry competitors are experiencing identical structural headwind problems.", score: 5, type: "Customer Satisfaction" },
                    { text: "Provide general operational details but decline to share financial metrics until next quarter.", score: 10, type: "Cost Control" }
                ]
            },
            {
                q: "Your production team requests permission to build out expensive local safety stock buffers, directly breaking your multi-year Lean/Just-In-Time operational mandate. What do you do?",
                options: [
                    { text: "Approve dynamic, risk-adjusted safety inventory buffers for high-risk strategic items.", score: 20, type: "Risk Management" },
                    { text: "Deny the request entirely to protect corporate free cash flow metrics and working capital efficiency.", score: 5, type: "Cost Control" },
                    { text: "Approve an immediate blanket doubling of all inventory stock components across the entire global organization.", score: 10, type: "Resilience" },
                    { text: "Order an extended 60-day cross-departmental assessment study before modifying inventory parameters.", score: 5, type: "Customer Satisfaction" }
                ]
            },
            {
                q: "A key regional distributor offers an exclusive premium contract if you shift your limited available inventory allocations entirely to them, leaving smaller tier-2 clients unsupplied.",
                options: [
                    { text: "Politely decline, honoring baseline commitments across channels to protect long-term brand equity.", score: 20, type: "Customer Satisfaction" },
                    { text: "Accept the deal immediately to optimize short-term revenue and lock in high margin profits.", score: 10, type: "Cost Control" },
                    { text: "Accept but covertly delay smaller accounts without proactively notifying them.", score: 0, type: "Risk Management" },
                    { text: "Propose an auction system where inventory goes to the highest bidder.", score: 5, type: "Resilience" }
                ]
            },
            {
                q: "Post-disruption analysis reveals that your procurement team relied entirely on single-source suppliers for critical paths due to low unit pricing. What mandate do you issue?",
                options: [
                    { text: "Mandate a dual or multi-sourcing strategy for all critical components, trading off minor margin for resilience.", score: 20, type: "Risk Management" },
                    { text: "Retain the single-source setups but negotiate stiffer financial penalty clauses for future supplier failures.", score: 8, type: "Cost Control" },
                    { text: "Insource all critical raw component manufacturing entirely, requiring heavy capital factory builds.", score: 12, type: "Resilience" },
                    { text: "Keep the current setup unchanged but expand administrative auditing frequency to spot issues early.", score: 5, type: "Customer Satisfaction" }
                ]
            },
            {
                q: "Your internal teams are burned out from manual crisis fire-fighting. They demand clear corporate policy infrastructure for future disruptions. What is your path?",
                options: [
                    { text: "Establish a permanent, cross-functional Supply Chain Risk Management (SCRM) team with automated monitoring tools.", score: 20, type: "Resilience" },
                    { text: "Advise teams that unpredictability is normal and recommend ad-hoc taskforces when crises occur.", score: 0, type: "Risk Management" },
                    { text: "Increase departmental overtime pay budgets without altering core risk handling frameworks.", score: 5, type: "Cost Control" },
                    { text: "Hire an external advisory consultancy firm to draft structural guide handbooks for future reference.", score: 12, type: "Customer Satisfaction" }
                ]
            }
        ];

        const AI_INVESTMENTS = [
            { id: "demand", name: "Predictive Machine-Learning Demand Forecasting", impact: "Eliminates bullwhip amplification by parsing multi-channel consumer datasets to forecast real inventory requirements accurately.", target: "Customer Satisfaction" },
            { id: "inventory", name: "Dynamic Multi-Echelon Inventory Optimization", impact: "Uses advanced math modeling to calculate the exact optimal positioning of safety buffers across regional warehouses.", target: "Cost Control" },
            { id: "risk", name: "Real-Time Supplier Risk & Geospatial Monitoring", impact: "Ingests live global news, weather feeds, and port logs to flag incoming tier-supplier disruptions days before they occur.", target: "Risk Management" },
            { id: "vision", name: "Computer Vision & Automated Warehouse Logistics", impact: "Deploys warehouse optical tracking and robotics to slash picking labor dependency and optimize storage velocity.", target: "Resilience" },
            { id: "copilot", name: "Generative AI Procurement Copilot", impact: "Automates complex master contract parsing, instant RFQ drafting, and spot-market vendor negotiations within minutes.", target: "Negotiation" }
        ];

        // --- STYLES ---
        const styles = {
            appContainer: { maxWidth: '1100px', margin: '0 auto', padding: '40px 20px', width: '100%' },
            card: { backgroundColor: 'var(--bg-card)', border: '1px solid var(--border-color)', borderRadius: '16px', padding: '32px', marginBottom: '24px', transition: 'all 0.3s ease' },
            btn: { background: 'var(--accent-blue)', color: '#fff', border: 'none', borderRadius: '8px', padding: '14px 28px', fontSize: '16px', fontWeight: '600', cursor: 'pointer', transition: 'all 0.2s ease', display: 'inline-flex', alignItems: 'center', justifyContent: 'center', gap: '8px' },
            btnSecondary: { background: 'transparent', color: 'var(--text-primary)', border: '1px solid var(--border-color)', borderRadius: '8px', padding: '14px 28px', fontSize: '16px', fontWeight: '600', cursor: 'pointer', transition: 'all 0.2s ease' },
            progressOuter: { width: '100%', backgroundColor: 'rgba(36, 50, 79, 0.5)', borderRadius: '999px', height: '10px', overflow: 'hidden', marginTop: '8px' },
            grid: { display: 'grid', gridTemplateColumns: 'repeat(auto-fit, minmax(280px, 1fr))', gap: '20px', marginBottom: '24px' },
            badge: (type) => {
                let bg = 'rgba(59, 130, 246, 0.15)', color = 'var(--accent-blue)';
                if (type === 'CRITICAL' || type === 'high-cost') { bg = 'rgba(239, 68, 68, 0.15)'; color = 'var(--accent-red)'; }
                if (type === 'HIGH') { bg = 'rgba(245, 158, 11, 0.15)'; color = 'var(--accent-yellow)'; }
                return { display: 'inline-block', padding: '6px 12px', borderRadius: '6px', fontSize: '12px', fontWeight: '700', backgroundColor: bg, color: color, textTransform: 'uppercase', letterSpacing: '0.5px' };
            }
        };

        // --- COMPONENT: PROGRESS BAR ---
        function MetricBar({ label, value, color = 'var(--accent-blue)' }) {
            const clampedValue = Math.max(0, Math.min(100, value));
            return (
                <div style={{ marginBottom: '16px' }}>
                    <div style={{ display: 'flex', justifyContent: 'space-between', fontSize: '14px', marginBottom: '4px' }}>
                        <span style={{ color: 'var(--text-secondary)', fontWeight: '500' }}>{label}</span>
                        <span style={{ fontWeight: '700', color: color }}>{clampedValue}%</span>
                    </div>
                    <div style={styles.progressOuter}>
                        <div style={{ width: `${clampedValue}%`, backgroundColor: color, height: '100%', borderRadius: '999px', transition: 'width 0.6s cubic-bezier(0.4, 0, 0.2, 1)' }} />
                    </div>
                </div>
            );
        }

        // --- MAIN APP ---
        function App() {
            const [step, setStep] = useState(1);
            const [company, setCompany] = useState(null);
            const [crisis, setCrisis] = useState(null);
            
            // Simulation Operational Metrics
            const [metrics, setMetrics] = useState({ cost: 40, inventory: 50, profit: 75, speed: 70, satisfaction: 80 });
            
            // Stage States
            const [selectedActions, setSelectedActions] = useState([]);
            const [negRound, setNegRound] = useState(0);
            const [negStats, setNegStats] = useState({ trust: 50, price: 50, leadTime: 50 });
            const [boardroomRound, setBoardroomRound] = useState(0);
            const [scores, setScores] = useState({ leadership: 50, negotiation: 50, resilience: 50, costControl: 50, riskMgmt: 50 });
            const [selectedAI, setSelectedAI] = useState([]);

            // Logs for Final Feedback
            const [decisionsMade, setDecisionsMade] = useState({ best: "", worst: "" });

            // Generate Fictional Company & Crisis
            const initializeSimulation = () => {
                const ind = INDUSTRIES[Math.floor(Math.random() * INDUSTRIES.length)];
                const crs = CRISES[Math.floor(Math.random() * CRISES.length)];
                
                const generatedCompany = {
                    industry: ind.name,
                    description: ind.desc,
                    revenue: `$${(Math.random() * 4 + 1.5).toFixed(1)}B ARR`,
                    factories: `${Math.floor(Math.random() * 3 + 2)} Core Production Plants`,
                    warehouses: `${Math.floor(Math.random() * 5 + 4)} Regional Fulfillment Hubs`,
                    suppliers: `${Math.floor(Math.random() * 40 + 20)} Tier-1 Standard Vendors`,
                    inventoryDays: `${Math.floor(Math.random() * 20 + 15)} Days of Supply (Safety Stock)`,
                    leadTime: `${Math.floor(Math.random() * 30 + 45)} Days Standard Lead Time`,
                    countries: "Spanned across North America, Eurozone, and APAC corridors"
                };

                setCompany(generatedCompany);
                setCrisis(crs);
                
                // Set initial metrics with a slight hit from the crisis
                setMetrics({
                    cost: 50 + Math.abs(crs.metricsEffect.cost * 0.3),
                    inventory: 50 + crs.metricsEffect.inventory * 0.3,
                    profit: 70 + crs.metricsEffect.profit * 0.3,
                    speed: 75 + crs.metricsEffect.speed * 0.4,
                    satisfaction: 80 + crs.metricsEffect.satisfaction * 0.3
                });

                setStep(2);
            };

            const handleActionSelect = (action) => {
                if (selectedActions.includes(action.id)) {
                    setSelectedActions(selectedActions.filter(id => id !== action.id));
                } else {
                    if (selectedActions.length < 3) {
                        setSelectedActions([...selectedActions, action.id]);
                    }
                }
            };

            const processWarRoom = () => {
                let updated = { ...metrics };
                let bestText = "";
                let worstText = "";

                selectedActions.forEach(id => {
                    const opt = WAR_ROOM_OPTIONS.find(o => o.id === id);
                    updated.cost += opt.cost;
                    updated.inventory += opt.inventory;
                    updated.speed += opt.speed;
                    updated.satisfaction += opt.satisfaction;
                    updated.profit += opt.profit;

                    if (opt.id === 1 || opt.id === 6) bestText = opt.label;
                    if (opt.id === 5) worstText = opt.label;
                });

                setMetrics(updated);
                setDecisionsMade({
                    best: bestText || "Proactive Specification Alternative Engineering",
                    worst: worstText || "Absorbing extended disruption delays without alternative routing"
                });
                setStep(5); // Jump into Supplier Negotiation
            };

            const handleNegotiationChoice = (choice) => {
                const nextTrust = Math.max(0, Math.min(100, negStats.trust + choice.trust));
                const nextPrice = Math.max(0, Math.min(100, negStats.price + choice.price));
                const nextLeadTime = Math.max(0, Math.min(100, negStats.leadTime + choice.leadTime));

                setNegStats({ trust: nextTrust, price: nextPrice, leadTime: nextLeadTime });

                if (negRound + 1 < NEGOTIATION_ROUNDS.length) {
                    setNegRound(negRound + 1);
                } else {
                    // Calculate Negotiation Score: High trust, controlled price, shorter lead time
                    const finalNegScore = Math.round((nextTrust + (100 - nextPrice) + (100 - nextLeadTime)) / 3);
                    setScores(prev => ({ ...prev, negotiation: finalNegScore, costControl: Math.round((prev.costControl + (100 - nextPrice)) / 2) }));
                    setStep(6); // Go to CEO Boardroom
                }
            };

            const handleBoardroomChoice = (choice) => {
                setScores(prev => {
                    const currentTypeVal = prev[choice.type.charAt(0).toLowerCase() + choice.type.slice(1).replace(' ', '')] || 50;
                    return {
                        ...prev,
                        leadership: prev.leadership + Math.round(choice.score * 0.2),
                        [choice.type.charAt(0).toLowerCase() + choice.type.slice(1).replace(' ', '')]: Math.round((currentTypeVal + choice.score) / 2)
                    };
                });

                if (boardroomRound + 1 < BOARDROOM_QUESTIONS.length) {
                    setBoardroomRound(boardroomRound + 1);
                } else {
                    setStep(7); // Go to AI Strategy Selection
                }
            };

            const handleAIToggle = (id) => {
                if (selectedAI.includes(id)) {
                    setSelectedAI(selectedAI.filter(item => item !== id));
                } else {
                    if (selectedAI.length < 2) {
                        setSelectedAI([...selectedAI, id]);
                    }
                }
            };

            const processAIStrategy = () => {
                // Adjust scores based on selected AI modules
                selectedAI.forEach(id => {
                    const ai = AI_INVESTMENTS.find(a => a.id === id);
                    if (ai.target === "Customer Satisfaction") setMetrics(p => ({ ...p, satisfaction: Math.min(100, p.satisfaction + 15) }));
                    if (ai.target === "Cost Control") setScores(p => ({ ...p, costControl: Math.min(100, p.costControl + 15) }));
                    if (ai.target === "Risk Management") setScores(p => ({ ...p, riskMgmt: Math.min(100, p.riskMgmt + 15) }));
                    if (ai.target === "Resilience") setScores(p => ({ ...p, resilience: Math.min(100, p.resilience + 15) }));
                    if (ai.target === "Negotiation") setScores(p => ({ ...p, negotiation: Math.min(100, p.negotiation + 15) }));
                });
                setStep(8); // Display Final Results Dashboard
            };

            // Calculate overall framework crisis management index
            const getOverallScore = () => {
                const operationalComponent = (metrics.satisfaction + metrics.speed + (100 - metrics.cost) + metrics.profit) / 4;
                const strategicComponent = (scores.leadership + scores.negotiation + scores.resilience + scores.riskMgmt + scores.costControl) / 5;
                return Math.round(Math.max(0, Math.min(100, (operationalComponent + strategicComponent) / 2)));
            };

            const resetSimulation = () => {
                setStep(1);
                setSelectedActions([]);
                setNegRound(0);
                setNegStats({ trust: 50, price: 50, leadTime: 50 });
                setBoardroomRound(0);
                setSelectedAI([]);
                setScores({ leadership: 50, negotiation: 50, resilience: 50, costControl: 50, riskMgmt: 50 });
            };

            return (
                <div style={styles.appContainer} className="fade-in">
                    {/* Header bar */}
                    <div style={{ display: 'flex', justifyContent: 'between', alignItems: 'center', marginBottom: '40px', borderBottom: '1px solid var(--border-color)', paddingBottom: '20px' }}>
                        <div>
                            <h1 style={{ fontSize: '24px', fontWeight: '700', letterSpacing: '-0.5px' }}>OPERATION LIFELINE</h1>
                            <p style={{ fontSize: '14px', color: 'var(--text-secondary)' }}>Enterprise Supply Chain Crisis Simulator</p>
                        </div>
                        <div style={{ marginLeft: 'auto', textAlign: 'right' }}>
                            <span style={styles.badge('HIGH')}>Simulation Status: Active</span>
                        </div>
                    </div>

                    {/* STEP 1: WELCOME SCREEN */}
                    {step === 1 && (
                        <div style={styles.card} className="fade-in">
                            <h2 style={{ fontSize: '36px', fontWeight: '700', marginBottom: '16px', lineHeight: '1.2' }}>Lead an Enterprise Through Operational Chaos</h2>
                            <p style={{ color: 'var(--text-secondary)', fontSize: '16px', lineHeight: '1.6', marginBottom: '24px' }}>
                                Welcome to the supply chain crisis lab. Global infrastructure is complex, delicate, and prone to rapid disruptions. 
                                In this simulator, you will take command of an international enterprise at the precise moment its primary supplier line fractures. 
                                Your mission is to balance cost containment, maintain delivery speeds, and shield customer satisfaction through high-stakes negotiations, 
                                tactical war-room maneuvering, and strategic board leadership.
                            </p>
                            <div style={{ backgroundColor: 'rgba(59, 130, 246, 0.05)', borderLeft: '4px solid var(--accent-blue)', padding: '16px', borderRadius: '4px', marginBottom: '32px' }}>
                                <p style={{ fontSize: '14px', color: 'var(--text-primary)', fontWeight: '500' }}>
                                    💡 <strong>Designed for Beginners:</strong> Every choice features a comprehensive context panel outlining why the operational mechanism matters to real-world industrial systems.
                                </p>
                            </div>
                            <button style={styles.btn} onClick={initializeSimulation}>Initialize Simulation Lab &rarr;</button>
                        </div>
                    )}

                    {/* STEP 2: COMPANY PROFILE & CRISIS MANIFEST */}
                    {step === 2 && (
                        <div className="fade-in">
                            <h2 style={{ fontSize: '22px', fontWeight: '600', marginBottom: '20px' }}>[Phase 01] System Parameter & Target Assessment</h2>
                            
                            <div style={styles.card}>
                                <span style={styles.badge('Enterprise Profile')}>Corporate Parameters</span>
                                <h3 style={{ fontSize: '28px', fontWeight: '700', marginTop: '12px', marginBottom: '8px' }}>{company.industry}</h3>
                                <p style={{ color: 'var(--text-secondary)', marginBottom: '24px' }}>{company.description}</p>
                                
                                <div style={styles.grid}>
                                    <div style={{ background: 'rgba(255,255,255,0.02)', padding: '16px', borderRadius: '8px', border: '1px solid var(--border-color)' }}>
                                        <div style={{ fontSize: '12px', color: 'var(--text-secondary)' }}>ANNUAL REVENUE</div>
                                        <div style={{ fontSize: '18px', fontWeight: '700', color: 'var(--accent-green)', marginTop: '4px' }}>{company.revenue}</div>
                                    </div>
                                    <div style={{ background: 'rgba(255,255,255,0.02)', padding: '16px', borderRadius: '8px', border: '1px solid var(--border-color)' }}>
                                        <div style={{ fontSize: '12px', color: 'var(--text-secondary)' }}>MANUFACTURING FOOTPRINT</div>
                                        <div style={{ fontSize: '16px', fontWeight: '600', marginTop: '4px' }}>{company.factories}</div>
                                    </div>
                                    <div style={{ background: 'rgba(255,255,255,0.02)', padding: '16px', borderRadius: '8px', border: '1px solid var(--border-color)' }}>
                                        <div style={{ fontSize: '12px', color: 'var(--text-secondary)' }}>DISTRIBUTION ECOSYSTEM</div>
                                        <div style={{ fontSize: '16px', fontWeight: '600', marginTop: '4px' }}>{company.warehouses}</div>
                                    </div>
                                    <div style={{ background: 'rgba(255,255,255,0.02)', padding: '16px', borderRadius: '8px', border: '1px solid var(--border-color)' }}>
                                        <div style={{ fontSize: '12px', color: 'var(--text-secondary)' }}>BUFFER SAFETY CAP</div>
                                        <div style={{ fontSize: '16px', fontWeight: '600', marginTop: '4px', color: 'var(--accent-yellow)' }}>{company.inventoryDays}</div>
                                    </div>
                                </div>
                                <p style={{ fontSize: '13px', color: 'var(--text-secondary)' }}>🌐 <strong>Geographic footprint:</strong> {company.countries}</p>
                            </div>

                            <div style={{ ...styles.card, border: '1px solid rgba(239, 68, 68, 0.4)', backgroundColor: 'rgba(239, 68, 68, 0.02)' }}>
                                <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center', marginBottom: '16px' }}>
                                    <span style={styles.badge(crisis.urgency)}>INCIDENT REPORT STATUS: UNCONTROLLED</span>
                                    <span style={{ fontSize: '14px', fontWeight: '600', color: 'var(--accent-red)' }}>CRISIS TRIGGERED</span>
                                </div>
                                <h3 style={{ fontSize: '24px', fontWeight: '700', marginBottom: '12px' }}>{crisis.title}</h3>
                                <p style={{ color: 'var(--text-primary)', lineHeight: '1.6', marginBottom: '16px' }}>{crisis.desc}</p>
                                <p style={{ color: 'var(--accent-red)', fontSize: '14px', fontWeight: '600' }}>⚠️ <strong>Immediate Business Shock:</strong> {crisis.impact}</p>
                            </div>

                            <div style={{ display: 'flex', justifyContent: 'flex-end' }}>
                                <button style={styles.btn} onClick={() => setStep(3)}>Enter Operational War Room &rarr;</button>
                            </div>
                        </div>
                    )}

                    {/* STEP 3: WAR ROOM ACTIONS SELECT */}
                    {step === 3 && (
                        <div className="fade-in">
                            <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center', marginBottom: '24px' }}>
                                <div>
                                    <h2 style={{ fontSize: '22px', fontWeight: '600' }}>[Phase 02] Crisis War Room Containment Strategy</h2>
                                    <p style={{ color: 'var(--text-secondary)', fontSize: '14px' }}>Select exactly 3 critical interventions to stabilize structural metrics.</p>
                                </div>
                                <div style={{ fontSize: '14px', fontWeight: '600', color: 'var(--accent-blue)' }}>
                                    Selected: {selectedActions.length} / 3
                                </div>
                            </div>

                            <div style={styles.grid}>
                                {WAR_ROOM_OPTIONS.map(opt => {
                                    const isSelected = selectedActions.includes(opt.id);
                                    return (
                                        <div 
                                            key={opt.id} 
                                            onClick={() => handleActionSelect(opt)}
                                            style={{ 
                                                ...styles.card, 
                                                marginBottom: '0',
                                                cursor: 'pointer', 
                                                borderColor: isSelected ? 'var(--accent-blue)' : 'var(--border-color)',
                                                backgroundColor: isSelected ? 'rgba(59, 130, 246, 0.08)' : 'var(--bg-card)',
                                                transform: isSelected ? 'translateY(-2px)' : 'none'
                                            }}
                                        >
                                            <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'flex-start', marginBottom: '12px' }}>
                                                <h4 style={{ fontSize: '16px', fontWeight: '600', maxWidth: '80%' }}>{opt.label}</h4>
                                                <div style={{ width: '20px', height: '20px', borderRadius: '50%', border: '2px solid var(--border-color)', display: 'flex', alignItems: 'center', justifyContent: 'center', backgroundColor: isSelected ? 'var(--accent-blue)' : 'transparent' }}>
                                                    {isSelected && <span style={{ color: '#fff', fontSize: '12px' }}>✓</span>}
                                                </div>
                                            </div>
                                            <p style={{ fontSize: '13px', color: 'var(--text-secondary)', lineHeight: '1.5', marginBottom: '16px' }}>{opt.why}</p>
                                            <div style={{ display: 'flex', flexWrap: 'wrap', gap: '6px', fontSize: '11px', fontWeight: '600' }}>
                                                <span style={{ padding: '3px 6px', borderRadius: '4px', background: 'rgba(255,255,255,0.05)' }}>Cost: {opt.cost > 0 ? `+${opt.cost}` : opt.cost}</span>
                                                <span style={{ padding: '3px 6px', borderRadius: '4px', background: 'rgba(255,255,255,0.05)' }}>Inventory: {opt.inventory > 0 ? `+${opt.inventory}` : opt.inventory}</span>
                                                <span style={{ padding: '3px 6px', borderRadius: '4px', background: 'rgba(255,255,255,0.05)' }}>Speed: {opt.speed > 0 ? `+${opt.speed}` : opt.speed}</span>
                                            </div>
                                        </div>
                                    );
                                })}
                            </div>

                            <div style={{ display: 'flex', justifyContent: 'flex-end' }}>
                                <button 
                                    style={{ ...styles.btn, opacity: selectedActions.length === 3 ? 1 : 0.5, cursor: selectedActions.length === 3 ? 'pointer' : 'not-allowed' }} 
                                    disabled={selectedActions.length !== 3}
                                    onClick={() => setStep(4)}
                                >
                                    Review Strategy Consequences &rarr;
                                </button>
                            </div>
                        </div>
                    )}

                    {/* STEP 4: WAR ROOM CONSEQUENCES VIEW */}
                    {step === 4 && (
                        <div className="fade-in" style={styles.card}>
                            <h2 style={{ fontSize: '22px', fontWeight: '600', marginBottom: '16px' }}>[Phase 02] Strategic Interventions Evaluation</h2>
                            <p style={{ color: 'var(--text-secondary)', marginBottom: '24px' }}>
                                Your selected tactical overrides have been rolled out across global nodes. See how the operational ecosystem adapts below.
                            </p>

                            <div style={{ ...styles.grid, gridTemplateColumns: '1fr 1fr', gap: '40px', marginBottom: '32px' }}>
                                <div>
                                    <h3 style={{ fontSize: '16px', fontWeight: '600', marginBottom: '16px', color: 'var(--accent-blue)' }}>Updated System Metrics Tracking</h3>
                                    <MetricBar label="Supply Chain Overhead Costs" value={metrics.cost} color="var(--accent-red)" />
                                    <MetricBar label="Ecosystem Pipeline Inventory Volume" value={metrics.inventory} color="var(--accent-purple)" />
                                    <MetricBar label="Logistics Logistics Flow Speed" value={metrics.speed} color="var(--accent-green)" />
                                    <MetricBar label="Customer Satisfaction Sentiment" value={metrics.satisfaction} color="var(--accent-blue)" />
                                </div>
                                <div style={{ background: 'rgba(255,255,255,0.01)', border: '1px solid var(--border-color)', padding: '24px', borderRadius: '12px' }}>
                                    <h4 style={{ fontSize: '14px', fontWeight: '600', marginBottom: '12px' }}>War Room Analysis Insights</h4>
                                    <p style={{ fontSize: '14px', color: 'var(--text-secondary)', lineHeight: '1.6' }}>
                                        By combining your choice allocations, you have altered the core flow mechanics. 
                                        High logistics overhead prices reflect rapid expedited freight use, but your network has managed to maintain basic shipment availability to avoid total contractual breach penalties.
                                    </p>
                                </div>
                            </div>

                            <div style={{ display: 'flex', justifyContent: 'flex-end' }}>
                                <button style={styles.btn} onClick={processWarRoom}>Proceed to Live Supplier Negotiation &rarr;</button>
                            </div>
                        </div>
                    )}

                    {/* STEP 5: BRANCHING SUPPLIER NEGOTIATION */}
                    {step === 5 && (
                        <div className="fade-in" style={styles.card}>
                            <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center', marginBottom: '20px', borderBottom: '1px solid var(--border-color)', paddingBottom: '16px' }}>
                                <div>
                                    <span style={styles.badge('Interactive Round')}>High-Stakes Contract Refactoring</span>
                                    <h2 style={{ fontSize: '22px', fontWeight: '600', marginTop: '6px' }}>Round {NEGOTIATION_ROUNDS[negRound].round} of 4</h2>
                                </div>
                                <div style={{ textAlign: 'right' }}>
                                    <span style={{ fontSize: '12px', color: 'var(--text-secondary)', block: 'block' }}>SUPPLIER RELATIONSHIP METRICS</span>
                                    <div style={{ display: 'flex', gap: '16px', fontSize: '14px', fontWeight: '600', marginTop: '4px' }}>
                                        <span style={{ color: 'var(--accent-blue)' }}>Trust: {negStats.trust}%</span>
                                        <span style={{ color: 'var(--accent-red)' }}>Premium Price: {negStats.price}%</span>
                                        <span style={{ color: 'var(--accent-yellow)' }}>Lead Time: {negStats.leadTime}%</span>
                                    </div>
                                </div>
                            </div>

                            <p style={{ fontSize: '16px', fontWeight: '500', lineHeight: '1.6', marginBottom: '24px', color: 'var(--text-primary)' }}>
                                {NEGOTIATION_ROUNDS[negRound].prompt}
                            </p>

                            <div style={{ display: 'flex', flexDirection: 'column', gap: '12px' }}>
                                {NEGOTIATION_ROUNDS[negRound].choices.map((choice, idx) => (
                                    <button 
                                        key={idx} 
                                        style={{ ...styles.btnSecondary, textAlign: 'left', display: 'block', width: '100%', padding: '16px', hover: { backgroundColor: 'var(--bg-card-hover)' } }}
                                        onClick={() => handleNegotiationChoice(choice)}
                                        onMouseEnter={(e) => e.target.style.backgroundColor = 'var(--bg-card-hover)'}
                                        onMouseLeave={(e) => e.target.style.backgroundColor = 'transparent'}
                                    >
                                        {choice.text}
                                    </button>
                                ))}
                            </div>
                        </div>
                    )}

                    {/* STEP 6: CEO BOARDROOM QUESTIONS */}
                    {step === 6 && (
                        <div className="fade-in" style={styles.card}>
                            <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center', marginBottom: '20px', borderBottom: '1px solid var(--border-color)', paddingBottom: '16px' }}>
                                <div>
                                    <span style={styles.badge('Executive Leadership')}>Executive Boardroom Strategy Review</span>
                                    <h2 style={{ fontSize: '22px', fontWeight: '600', marginTop: '6px' }}>Governance Evaluation {boardroomRound + 1} of 5</h2>
                                </div>
                                <div style={{ fontSize: '14px', fontWeight: '600', color: 'var(--accent-purple)' }}>
                                    Leadership Index: {scores.leadership}%
                                </div>
                            </div>

                            <p style={{ fontSize: '16px', fontWeight: '500', lineHeight: '1.6', marginBottom: '24px', color: 'var(--text-primary)' }}>
                                {BOARDROOM_QUESTIONS[boardroomRound].q}
                            </p>

                            <div style={{ display: 'flex', flexDirection: 'column', gap: '12px' }}>
                                {BOARDROOM_QUESTIONS[boardroomRound].options.map((option, idx) => (
                                    <button 
                                        key={idx} 
                                        style={{ ...styles.btnSecondary, textAlign: 'left', display: 'block', width: '100%', padding: '16px' }}
                                        onClick={() => handleBoardroomChoice(option)}
                                        onMouseEnter={(e) => e.target.style.backgroundColor = 'var(--bg-card-hover)'}
                                        onMouseLeave={(e) => e.target.style.backgroundColor = 'transparent'}
                                    >
                                        {option.text}
                                    </button>
                                ))}
                            </div>
                        </div>
                    )}

                    {/* STEP 7: AI STRATEGY ROADMAP INVESTMENTS */}
                    {step === 7 && (
                        <div className="fade-in">
                            <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center', marginBottom: '24px' }}>
                                <div>
                                    <h2 style={{ fontSize: '22px', fontWeight: '600' }}>[Phase 04] Modern AI Architecture Transformation Strategy</h2>
                                    <p style={{ color: 'var(--text-secondary)', fontSize: '14px' }}>Select up to 2 digital AI solutions to fortify operations against future volatility shocks.</p>
                                </div>
                                <div style={{ fontSize: '14px', fontWeight: '600', color: 'var(--accent-purple)' }}>
                                    Selected: {selectedAI.length} / 2
                                </div>
                            </div>

                            <div style={styles.grid}>
                                {AI_INVESTMENTS.map(ai => {
                                    const isSelected = selectedAI.includes(ai.id);
                                    return (
                                        <div 
                                            key={ai.id} 
                                            onClick={() => handleAIToggle(ai.id)}
                                            style={{ 
                                                ...styles.card, 
                                                marginBottom: '0',
                                                cursor: 'pointer', 
                                                borderColor: isSelected ? 'var(--accent-purple)' : 'var(--border-color)',
                                                backgroundColor: isSelected ? 'rgba(139, 92, 246, 0.08)' : 'var(--bg-card)',
                                                transform: isSelected ? 'translateY(-2px)' : 'none'
                                            }}
                                        >
                                            <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'flex-start', marginBottom: '12px' }}>
                                                <h4 style={{ fontSize: '16px', fontWeight: '600', color: '#fff' }}>{ai.name}</h4>
                                                <div style={{ width: '20px', height: '20px', borderRadius: '50%', border: '2px solid var(--border-color)', display: 'flex', alignItems: 'center', justifyContent: 'center', backgroundColor: isSelected ? 'var(--accent-purple)' : 'transparent' }}>
                                                    {isSelected && <span style={{ color: '#fff', fontSize: '12px' }}>✓</span>}
                                                </div>
                                            </div>
                                            <p style={{ fontSize: '13px', color: 'var(--text-secondary)', lineHeight: '1.5', marginBottom: '12px' }}>{ai.impact}</p>
                                            <span style={{ fontSize: '11px', fontWeight: '600', color: 'var(--accent-purple)', textTransform: 'uppercase' }}>Targets: {ai.target}</span>
                                        </div>
                                    );
                                })}
                            </div>

                            <div style={{ display: 'flex', justifyContent: 'flex-end' }}>
                                <button 
                                    style={{ ...styles.btn, background: 'var(--accent-purple)', opacity: selectedAI.length === 2 ? 1 : 0.5, cursor: selectedAI.length === 2 ? 'pointer' : 'not-allowed' }} 
                                    disabled={selectedAI.length !== 2}
                                    onClick={processAIStrategy}
                                >
                                    Generate Final Performance Audit &rarr;
                                </button>
                            </div>
                        </div>
                    )}

                    {/* STEP 8: FINAL SUMMARY RESULTS DASHBOARD */}
                    {step === 8 && (
                        <div className="fade-in">
                            <div style={styles.card}>
                                <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center', marginBottom: '24px', flexWrap: 'wrap', gap: '16px' }}>
                                    <div>
                                        <span style={styles.badge('Simulation Concluded')}>Crisis Post-Mortem Manifest</span>
                                        <h2 style={{ fontSize: '32px', fontWeight: '700', marginTop: '6px' }}>Overall Crisis Score: {getOverallScore()} / 100</h2>
                                    </div>
                                    <button style={styles.btn} onClick={resetSimulation}>Replay Simulation Lab</button>
                                </div>

                                <div style={{ ...styles.grid, gridTemplateColumns: 'repeat(auto-fit, minmax(220px, 1fr))', marginBottom: '32px' }}>
                                    <div style={{ background: 'rgba(255,255,255,0.01)', padding: '16px', borderRadius: '8px', border: '1px solid var(--border-color)' }}>
                                        <MetricBar label="Executive Leadership" value={scores.leadership} color="var(--accent-blue)" />
                                    </div>
                                    <div style={{ background: 'rgba(255,255,255,0.01)', padding: '16px', borderRadius: '8px', border: '1px solid var(--border-color)' }}>
                                        <MetricBar label="Supplier Negotiation" value={scores.negotiation} color="var(--accent-green)" />
                                    </div>
                                    <div style={{ background: 'rgba(255,255,255,0.01)', padding: '16px', borderRadius: '8px', border: '1px solid var(--border-color)' }}>
                                        <MetricBar label="Structural Resilience" value={scores.resilience} color="var(--accent-purple)" />
                                    </div>
                                    <div style={{ background: 'rgba(255,255,255,0.01)', padding: '16px', borderRadius: '8px', border: '1px solid var(--border-color)' }}>
                                        <MetricBar label="Cost Containment" value={scores.costControl} color="var(--accent-red)" />
                                    </div>
                                </div>

                                <div style={{ borderTop: '1px solid var(--border-color)', paddingTop: '24px', marginTop: '24px' }}>
                                    <h3 style={{ fontSize: '18px', fontWeight: '600', marginBottom: '16px' }}>Personalized Executive Performance Assessment</h3>
                                    
                                    <div style={{ display: 'grid', gridTemplateColumns: '1fr 1fr', gap: '20px', marginBottom: '24px' }}>
                                        <div style={{ backgroundColor: 'rgba(16, 185, 129, 0.05)', borderLeft: '4px solid var(--accent-green)', padding: '16px', borderRadius: '4px' }}>
                                            <h4 style={{ fontSize: '14px', fontWeight: '600', color: 'var(--accent-green)', marginBottom: '4px' }}>🏆 Strongest Strategic Driver</h4>
                                            <p style={{ fontSize: '13px', color: 'var(--text-primary)' }}>{decisionsMade.best}</p>
                                        </div>
                                        <div style={{ backgroundColor: 'rgba(239, 68, 68, 0.05)', borderLeft: '4px solid var(--accent-red)', padding: '16px', borderRadius: '4px' }}>
                                            <h4 style={{ fontSize: '14px', fontWeight: '600', color: 'var(--accent-red)', marginBottom: '4px' }}>⚠️ Critical Weakness Exposure</h4>
                                            <p style={{ fontSize: '13px', color: 'var(--text-primary)' }}>{decisionsMade.worst || "High immediate spot market procurement price exposures"}</p>
                                        </div>
                                    </div>

                                    <div style={{ background: 'rgba(255,255,255,0.02)', padding: '20px', borderRadius: '8px', border: '1px solid var(--border-color)' }}>
                                        <h4 style={{ fontSize: '14px', fontWeight: '600', marginBottom: '8px' }}>Lessons Learned for Modern Global Sourcing</h4>
                                        <p style={{ fontSize: '14px', color: 'var(--text-secondary)', lineHeight: '1.6' }}>
                                            True modern supply resilience is never achieved by buying the cheapest component option. 
                                            By building diversified dual-sourcing options and integrating live algorithmic risk warning indicators, 
                                            enterprises can shield themselves from volatile market spikes and protect long-term market valuation during unexpected disruptions.
                                        </p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    )}
                </div>
            );
        }

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>