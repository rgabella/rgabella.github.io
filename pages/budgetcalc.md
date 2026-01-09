---
layout: page
title: Budget Calculator
permalink: /budgetcalc/
---

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Budget Calculator</title>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- React & ReactDOM -->
    <script crossorigin src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    
    <!-- Babel for JSX -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

    <style>
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
    </style>
</head>
<body class="bg-slate-100 min-h-screen">

    <div id="root"></div>

    <script type="text/babel">
        const { useState, useEffect } = React;

        // --- Icons (Inline SVGs to remove external dependencies) ---
        const PieChartIcon = ({ className }) => (
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}><path d="M21.21 15.89A10 10 0 1 1 8 2.83"/><path d="M22 12A10 10 0 0 0 12 2v10z"/></svg>
        );

        const WalletIcon = ({ size }) => (
            <svg xmlns="http://www.w3.org/2000/svg" width={size} height={size} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M20 12V8H6a2 2 0 0 1-2-2c0-1.1.9-2 2-2h12v4"/><path d="M4 6v12c0 1.1.9 2 2 2h14v-4"/><path d="M18 12a2 2 0 0 0-2 2c0 1.1.9 2 2 2h2v-4Z"/></svg>
        );

        const ShoppingBagIcon = ({ size }) => (
            <svg xmlns="http://www.w3.org/2000/svg" width={size} height={size} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M6 2 3 6v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2V6l-3-4Z"/><path d="M3 6h18"/><path d="M16 10a4 4 0 0 1-8 0"/></svg>
        );

        const PiggyBankIcon = ({ size }) => (
            <svg xmlns="http://www.w3.org/2000/svg" width={size} height={size} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M19 5c-1.5 0-2.8.6-3.8 1.5l-2.5 2.5c-.3.3-.7.6-1.2.6H8.3a2 2 0 0 0-1.7 2.6L8 16a2 2 0 0 0 1.8 1.4h.3L13 14l2.6 1.5c1 .6 2.4.4 3.3-.2l1.1-1.1a2 2 0 0 0 0-2.8l-1-1"/><path d="M2 20c.5-3.5 2.5-6.1 6.2-7.4a6 6 0 0 1 8.2 1.4"/><path d="M16 4a2 2 0 0 0 2 2"/></svg>
        );

        // --- Main Component ---
        const App = () => {
            const [inputValue, setInputValue] = useState('');
            const [amount, setAmount] = useState(0);

            // Update numeric amount when input changes
            useEffect(() => {
                const numericValue = parseFloat(inputValue.replace(/,/g, ''));
                setAmount(isNaN(numericValue) ? 0 : numericValue);
            }, [inputValue]);

            // Format currency to PHP
            const formatPHP = (val) => {
                return new Intl.NumberFormat('en-PH', {
                    style: 'currency',
                    currency: 'PHP',
                    minimumFractionDigits: 2
                }).format(val);
            };

            // Calculations
            const needs = amount * 0.50;
            const wants = amount * 0.30;
            const savings = amount * 0.20;

            const handleInputChange = (e) => {
                setInputValue(e.target.value);
            };

            return (
                <div className="py-8 px-4 font-sans text-slate-800">
                    <div className="max-w-md mx-auto space-y-6">
                        
                        {/* Header */}
                        <div className="text-center space-y-2">
                            <h1 className="text-3xl font-bold text-slate-900 flex items-center justify-center gap-2">
                                <PieChartIcon className="w-8 h-8 text-blue-600" />
                                Budget Calc
                            </h1>
                            <p className="text-slate-500">50/30/20 Rule Calculator</p>
                        </div>

                        {/* Input Card */}
                        <div className="bg-white p-6 rounded-2xl shadow-lg border border-slate-200">
                            <label className="block text-sm font-semibold text-slate-600 mb-2 uppercase tracking-wide">
                                Enter Total Income
                            </label>
                            <div className="relative">
                                <div className="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                                    <span className="text-slate-400 font-bold">₱</span>
                                </div>
                                <input
                                    type="number"
                                    value={inputValue}
                                    onChange={handleInputChange}
                                    placeholder="0.00"
                                    className="block w-full pl-8 pr-4 py-4 text-2xl font-bold text-slate-900 bg-slate-50 border-2 border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none transition-all placeholder:text-slate-300"
                                />
                            </div>
                        </div>

                        {/* Visual Bar */}
                        {amount > 0 && (
                            <div className="flex w-full h-4 rounded-full overflow-hidden shadow-inner bg-slate-200">
                                <div className="h-full bg-blue-500 transition-all duration-500" style={{ width: '50%' }}></div>
                                <div className="h-full bg-purple-500 transition-all duration-500" style={{ width: '30%' }}></div>
                                <div className="h-full bg-green-500 transition-all duration-500" style={{ width: '20%' }}></div>
                            </div>
                        )}

                        {/* Results Grid */}
                        <div className="grid gap-4">
                            
                            {/* 50% Needs */}
                            <div className="bg-white p-5 rounded-xl shadow-sm border-l-4 border-blue-500 flex items-center justify-between transition-transform hover:scale-[1.02]">
                                <div className="flex items-center gap-4">
                                    <div className="p-3 bg-blue-100 text-blue-600 rounded-full">
                                        <WalletIcon size={24} />
                                    </div>
                                    <div>
                                        <p className="text-xs font-bold text-blue-600 uppercase tracking-wider">Needs (50%)</p>
                                        <p className="text-xs text-slate-400">Rent, Bills, Groceries</p>
                                    </div>
                                </div>
                                <div className="text-right">
                                    <span className="block text-xl font-bold text-slate-800">
                                        {formatPHP(needs)}
                                    </span>
                                </div>
                            </div>

                            {/* 30% Wants */}
                            <div className="bg-white p-5 rounded-xl shadow-sm border-l-4 border-purple-500 flex items-center justify-between transition-transform hover:scale-[1.02]">
                                <div className="flex items-center gap-4">
                                    <div className="p-3 bg-purple-100 text-purple-600 rounded-full">
                                        <ShoppingBagIcon size={24} />
                                    </div>
                                    <div>
                                        <p className="text-xs font-bold text-purple-600 uppercase tracking-wider">Wants (30%)</p>
                                        <p className="text-xs text-slate-400">Dining, Hobbies, Shopping</p>
                                    </div>
                                </div>
                                <div className="text-right">
                                    <span className="block text-xl font-bold text-slate-800">
                                        {formatPHP(wants)}
                                    </span>
                                </div>
                            </div>

                            {/* 20% Savings */}
                            <div className="bg-white p-5 rounded-xl shadow-sm border-l-4 border-green-500 flex items-center justify-between transition-transform hover:scale-[1.02]">
                                <div className="flex items-center gap-4">
                                    <div className="p-3 bg-green-100 text-green-600 rounded-full">
                                        <PiggyBankIcon size={24} />
                                    </div>
                                    <div>
                                        <p className="text-xs font-bold text-green-600 uppercase tracking-wider">Savings (20%)</p>
                                        <p className="text-xs text-slate-400">Investments, Debt, Emergency</p>
                                    </div>
                                </div>
                                <div className="text-right">
                                    <span className="block text-xl font-bold text-slate-800">
                                        {formatPHP(savings)}
                                    </span>
                                </div>
                            </div>

                        </div>

                        <div className="text-center text-xs text-slate-400 mt-8">
                           Calculated in Philippine Peso (PHP)
                        </div>
                    </div>
                </div>
            );
        };

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
