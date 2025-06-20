<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Video Poker Assistant - Jacks or Better</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #0f172a 0%, #1e293b 50%, #334155 100%);
            color: #e2e8f0;
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
            padding: 20px;
            background: rgba(15, 23, 42, 0.8);
            border-radius: 15px;
            border: 1px solid rgba(148, 163, 184, 0.2);
            backdrop-filter: blur(10px);
        }

        .header h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            background: linear-gradient(135deg, #fbbf24, #f59e0b);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .header p {
            color: #94a3b8;
            font-size: 1.1rem;
        }

        .main-content {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
            margin-bottom: 30px;
        }

        @media (max-width: 768px) {
            .main-content {
                grid-template-columns: 1fr;
            }
        }

        .card-input-section, .results-section {
            background: rgba(15, 23, 42, 0.8);
            border-radius: 15px;
            padding: 25px;
            border: 1px solid rgba(148, 163, 184, 0.2);
            backdrop-filter: blur(10px);
        }

        .section-title {
            font-size: 1.4rem;
            margin-bottom: 20px;
            color: #fbbf24;
        }

        .hand-display {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            flex-wrap: wrap;
            justify-content: center;
        }

        .card {
            width: 80px;
            height: 110px;
            background: linear-gradient(145deg, #ffffff, #f1f5f9);
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.2rem;
            font-weight: bold;
            color: #1e293b;
            border: 2px solid transparent;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            user-select: none;
        }

        .card:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 15px rgba(0, 0, 0, 0.2);
        }

        .card.selected {
            border-color: #fbbf24;
            transform: translateY(-5px);
            box-shadow: 0 8px 20px rgba(251, 191, 36, 0.3);
        }

        .card.red {
            color: #dc2626;
        }

        .card.black {
            color: #1e293b;
        }

        .controls {
            display: flex;
            gap: 15px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .btn {
            padding: 12px 24px;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .btn-primary {
            background: linear-gradient(135deg, #fbbf24, #f59e0b);
            color: #1e293b;
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 15px rgba(251, 191, 36, 0.3);
        }

        .btn-secondary {
            background: rgba(148, 163, 184, 0.2);
            color: #e2e8f0;
            border: 1px solid rgba(148, 163, 184, 0.3);
        }

        .btn-secondary:hover {
            background: rgba(148, 163, 184, 0.3);
            transform: translateY(-2px);
        }

        .input-group {
            margin-bottom: 20px;
        }

        .input-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #cbd5e1;
        }

        .card-input {
            width: 100%;
            padding: 12px;
            border: 1px solid rgba(148, 163, 184, 0.3);
            border-radius: 8px;
            background: rgba(15, 23, 42, 0.5);
            color: #e2e8f0;
            font-size: 1rem;
            font-family: monospace;
        }

        .card-input:focus {
            outline: none;
            border-color: #fbbf24;
            box-shadow: 0 0 0 3px rgba(251, 191, 36, 0.1);
        }

        .results {
            max-height: 600px;
            overflow-y: auto;
        }

        .strategy-item {
            background: rgba(30, 41, 59, 0.6);
            border-radius: 8px;
            padding: 15px;
            margin-bottom: 12px;
            border-left: 4px solid transparent;
            transition: all 0.3s ease;
        }

        .strategy-item:hover {
            background: rgba(30, 41, 59, 0.8);
        }

        .strategy-item.best {
            border-left-color: #fbbf24;
            background: rgba(251, 191, 36, 0.1);
        }

        .strategy-rank {
            font-size: 0.9rem;
            color: #94a3b8;
            margin-bottom: 5px;
        }

        .strategy-hold {
            font-size: 1.1rem;
            font-weight: 600;
            margin-bottom: 8px;
            color: #e2e8f0;
        }

        .strategy-ev {
            font-size: 1.2rem;
            font-weight: bold;
            color: #fbbf24;
            margin-bottom: 8px;
        }

        .strategy-outcomes {
            font-size: 0.9rem;
            color: #cbd5e1;
            line-height: 1.4;
        }

        .recommendation {
            background: linear-gradient(135deg, rgba(251, 191, 36, 0.1), rgba(245, 158, 11, 0.1));
            border: 2px solid #fbbf24;
            border-radius: 12px;
            padding: 20px;
            margin-top: 20px;
            text-align: center;
        }

        .recommendation h3 {
            color: #fbbf24;
            margin-bottom: 10px;
            font-size: 1.3rem;
        }

        .recommendation-text {
            font-size: 1.1rem;
            font-weight: 600;
        }

        .payout-table {
            background: rgba(15, 23, 42, 0.8);
            border-radius: 15px;
            padding: 25px;
            border: 1px solid rgba(148, 163, 184, 0.2);
            backdrop-filter: blur(10px);
        }

        .payout-grid {
            display: grid;
            grid-template-columns: 1fr auto;
            gap: 10px;
            align-items: center;
        }

        .payout-hand {
            font-weight: 600;
        }

        .payout-value {
            text-align: right;
            color: #fbbf24;
            font-weight: bold;
        }

        .loading {
            text-align: center;
            padding: 40px;
            color: #94a3b8;
        }

        .error {
            background: rgba(239, 68, 68, 0.1);
            border: 1px solid rgba(239, 68, 68, 0.3);
            border-radius: 8px;
            padding: 15px;
            margin-bottom: 20px;
            color: #fca5a5;
        }

        /* Custom scrollbar */
        .results::-webkit-scrollbar {
            width: 8px;
        }

        .results::-webkit-scrollbar-track {
            background: rgba(15, 23, 42, 0.3);
            border-radius: 4px;
        }

        .results::-webkit-scrollbar-thumb {
            background: rgba(148, 163, 184, 0.3);
            border-radius: 4px;
        }

        .results::-webkit-scrollbar-thumb:hover {
            background: rgba(148, 163, 184, 0.5);
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>Video Poker Assistant</h1>
            <p>Jacks or Better - Optimal Strategy Calculator</p>
        </div>

        <div class="main-content">
            <div class="card-input-section">
                <h2 class="section-title">Your Hand</h2>
                
                <div class="hand-display" id="handDisplay">
                    <!-- Cards will be dynamically generated here -->
                </div>

                <div class="controls">
                    <button class="btn btn-primary" onclick="generateRandomHand()">Random Hand</button>
                    <button class="btn btn-secondary" onclick="clearHand()">Clear</button>
                    <button class="btn btn-primary" onclick="analyzeHand()">Analyze</button>
                </div>

                <div class="input-group">
                    <label for="manualInput">Manual Input (e.g., "QS 8D 8S AC 2H"):</label>
                    <input type="text" id="manualInput" class="card-input" placeholder="Enter 5 cards separated by spaces">
                    <button class="btn btn-secondary" onclick="parseManualInput()" style="margin-top: 10px; width: 100%;">Parse Hand</button>
                </div>
            </div>

            <div class="results-section">
                <h2 class="section-title">Strategy Analysis</h2>
                <div class="results" id="results">
                    <div class="loading">
                        Enter a hand and click "Analyze" to see optimal strategy recommendations.
                    </div>
                </div>
            </div>
        </div>

        <div class="payout-table">
            <h2 class="section-title">Payout Table (1 Unit Bet)</h2>
            <div class="payout-grid">
                <div class="payout-hand">Royal Flush</div>
                <div class="payout-value">800x</div>
                <div class="payout-hand">Straight Flush</div>
                <div class="payout-value">50x</div>
                <div class="payout-hand">Four of a Kind</div>
                <div class="payout-value">25x</div>
                <div class="payout-hand">Full House</div>
                <div class="payout-value">9x</div>
                <div class="payout-hand">Flush</div>
                <div class="payout-value">6x</div>
                <div class="payout-hand">Straight</div>
                <div class="payout-value">4x</div>
                <div class="payout-hand">Three of a Kind</div>
                <div class="payout-value">3x</div>
                <div class="payout-hand">Two Pairs</div>
                <div class="payout-value">2x</div>
                <div class="payout-hand">Jacks or Better</div>
                <div class="payout-value">1x</div>
            </div>
        </div>
    </div>

    <script>
        // Card and game logic
        class Card {
            constructor(rank, suit) {
                this.rank = rank;
                this.suit = suit;
                this.rankValue = this.getRankValue();
            }

            getRankValue() {
                const rankMap = {
                    '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9, '10': 10,
                    'J': 11, 'Q': 12, 'K': 13, 'A': 14
                };
                return rankMap[this.rank];
            }

            toString() {
                return `${this.rank}${this.suit}`;
            }

            isRed() {
                return this.suit === '♥' || this.suit === '♦';
            }
        }

        class HandEvaluator {
            static PAYOUTS = {
                'Royal Flush': 800,
                'Straight Flush': 50,
                'Four of a Kind': 25,
                'Full House': 9,
                'Flush': 6,
                'Straight': 4,
                'Three of a Kind': 3,
                'Two Pairs': 2,
                'Jacks or Better': 1,
                'High Card': 0
            };

            static parseCard(cardStr) {
                const suitMap = {
                    '♠': '♠', '♥': '♥', '♦': '♦', '♣': '♣',
                    'S': '♠', 'H': '♥', 'D': '♦', 'C': '♣',
                    's': '♠', 'h': '♥', 'd': '♦', 'c': '♣'
                };

                if (cardStr.length >= 2) {
                    const rank = cardStr.slice(0, -1);
                    const suitChar = cardStr.slice(-1);
                    const suit = suitMap[suitChar] || suitChar;
                    return new Card(rank, suit);
                }
                throw new Error(`Invalid card format: ${cardStr}`);
            }

            static isFlush(cards) {
                const suits = new Set(cards.map(card => card.suit));
                return suits.size === 1;
            }

            static isStraight(cards) {
                if (cards.length !== 5) return false;

                const values = cards.map(card => card.rankValue).sort((a, b) => a - b);
                
                // Check regular straight
                if (values.every((val, i) => i === 0 || val === values[i-1] + 1)) {
                    return true;
                }

                // Check wheel (A-2-3-4-5)
                if (JSON.stringify(values) === JSON.stringify([2, 3, 4, 5, 14])) {
                    return true;
                }

                return false;
            }

            static isRoyalFlush(cards) {
                if (!this.isFlush(cards) || !this.isStraight(cards)) return false;
                const ranks = new Set(cards.map(card => card.rank));
                return JSON.stringify([...ranks].sort()) === JSON.stringify(['10', 'A', 'J', 'K', 'Q']);
            }

            static getRankCounts(cards) {
                const counts = {};
                cards.forEach(card => {
                    counts[card.rank] = (counts[card.rank] || 0) + 1;
                });
                return counts;
            }

            static evaluateHand(cards) {
                if (cards.length !== 5) return ['High Card', 0];

                const isFlush = this.isFlush(cards);
                const isStraight = this.isStraight(cards);
                const isRoyal = this.isRoyalFlush(cards);

                const rankCounts = this.getRankCounts(cards);
                const countValues = Object.values(rankCounts).sort((a, b) => b - a);

                if (isRoyal) return ['Royal Flush', this.PAYOUTS['Royal Flush']];
                if (isFlush && isStraight) return ['Straight Flush', this.PAYOUTS['Straight Flush']];
                if (JSON.stringify(countValues) === JSON.stringify([4, 1])) return ['Four of a Kind', this.PAYOUTS['Four of a Kind']];
                if (JSON.stringify(countValues) === JSON.stringify([3, 2])) return ['Full House', this.PAYOUTS['Full House']];
                if (isFlush) return ['Flush', this.PAYOUTS['Flush']];
                if (isStraight) return ['Straight', this.PAYOUTS['Straight']];
                if (JSON.stringify(countValues) === JSON.stringify([3, 1, 1])) return ['Three of a Kind', this.PAYOUTS['Three of a Kind']];
                if (JSON.stringify(countValues) === JSON.stringify([2, 2, 1])) return ['Two Pairs', this.PAYOUTS['Two Pairs']];
                
                if (JSON.stringify(countValues) === JSON.stringify([2, 1, 1, 1])) {
                    for (const [rank, count] of Object.entries(rankCounts)) {
                        if (count === 2 && ['J', 'Q', 'K', 'A'].includes(rank)) {
                            return ['Jacks or Better', this.PAYOUTS['Jacks or Better']];
                        }
                    }
                }

                return ['High Card', this.PAYOUTS['High Card']];
            }
        }

        class VideoPokerAnalyzer {
            constructor() {
                this.deck = this.createDeck();
            }

            createDeck() {
                const ranks = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K', 'A'];
                const suits = ['♠', '♥', '♦', '♣'];
                const deck = [];
                for (const rank of ranks) {
                    for (const suit of suits) {
                        deck.push(new Card(rank, suit));
                    }
                }
                return deck;
            }

            generateRandomHand() {
                const shuffled = [...this.deck].sort(() => Math.random() - 0.5);
                return shuffled.slice(0, 5);
            }

            getRemainingDeck(dealtCards) {
                const dealtSet = new Set(dealtCards.map(card => card.toString()));
                return this.deck.filter(card => !dealtSet.has(card.toString()));
            }

            calculateHoldEV(dealtHand, holdIndices) {
                const heldCards = holdIndices.map(i => dealtHand[i]);
                const remainingDeck = this.getRemainingDeck(dealtHand);
                const cardsToDraw = 5 - heldCards.length;

                const outcomeCounts = {};
                let totalOutcomes = 0;
                let totalValue = 0;

                // Generate all combinations of draw cards
                const combinations = this.getCombinations(remainingDeck, cardsToDraw);
                
                for (const drawCards of combinations) {
                    const finalHand = [...heldCards, ...drawCards];
                    const [handType, payout] = HandEvaluator.evaluateHand(finalHand);
                    
                    outcomeCounts[handType] = (outcomeCounts[handType] || 0) + 1;
                    totalOutcomes++;
                    totalValue += payout;
                }

                const expectedValue = totalOutcomes > 0 ? totalValue / totalOutcomes : 0;
                return [expectedValue, outcomeCounts];
            }

            getCombinations(arr, k) {
                if (k === 0) return [[]];
                if (arr.length === 0) return [];
                
                const [first, ...rest] = arr;
                const withFirst = this.getCombinations(rest, k - 1).map(combo => [first, ...combo]);
                const withoutFirst = this.getCombinations(rest, k);
                
                return [...withFirst, ...withoutFirst];
            }

            analyzeHand(hand) {
                const strategies = [];

                // Generate all possible hold combinations
                for (let r = 0; r <= 5; r++) {
                    const combinations = this.getCombinations([0, 1, 2, 3, 4], r);
                    for (const holdIndices of combinations) {
                        const heldCards = holdIndices.map(i => hand[i]);
                        const [ev, outcomes] = this.calculateHoldEV(hand, holdIndices);

                        strategies.push({
                            holdIndices,
                            heldCards,
                            expectedValue: ev,
                            outcomes
                        });
                    }
                }

                strategies.sort((a, b) => b.expectedValue - a.expectedValue);
                return strategies;
            }
        }

        // Global variables
        let currentHand = [];
        let selectedCards = new Set();
        const analyzer = new VideoPokerAnalyzer();

        // UI Functions
        function displayHand(hand) {
            const handDisplay = document.getElementById('handDisplay');
            handDisplay.innerHTML = '';

            hand.forEach((card, index) => {
                const cardElement = document.createElement('div');
                cardElement.className = `card ${card.isRed() ? 'red' : 'black'}`;
                cardElement.textContent = card.toString();
                cardElement.onclick = () => toggleCardSelection(index);
                cardElement.id = `card-${index}`;
                handDisplay.appendChild(cardElement);
            });
        }

        function toggleCardSelection(index) {
            const cardElement = document.getElementById(`card-${index}`);
            if (selectedCards.has(index)) {
                selectedCards.delete(index);
                cardElement.classList.remove('selected');
            } else {
                selectedCards.add(index);
                cardElement.classList.add('selected');
            }
        }

        function generateRandomHand() {
            currentHand = analyzer.generateRandomHand();
            selectedCards.clear();
            displayHand(currentHand);
            document.getElementById('results').innerHTML = '<div class="loading">Click "Analyze" to see strategy recommendations for this hand.</div>';
        }

        function clearHand() {
            currentHand = [];
            selectedCards.clear();
            document.getElementById('handDisplay').innerHTML = '';
            document.getElementById('results').innerHTML = '<div class="loading">Enter a hand and click "Analyze" to see optimal strategy recommendations.</div>';
        }

        function parseManualInput() {
            const input = document.getElementById('manualInput').value.trim();
            if (!input) return;

            try {
                const cardStrings = input.split(/\s+/);
                if (cardStrings.length !== 5) {
                    throw new Error('Please enter exactly 5 cards.');
                }

                const hand = cardStrings.map(str => HandEvaluator.parseCard(str));
                
                // Check for duplicates
                const cardSet = new Set(hand.map(card => card.toString()));
                if (cardSet.size !== 5) {
                    throw new Error('Duplicate cards detected.');
                }

                currentHand = hand;
                selectedCards.clear();
                displayHand(currentHand);
                document.getElementById('results').innerHTML = '<div class="loading">Click "Analyze" to see strategy recommendations for this hand.</div>';
                
            } catch (error) {
                document.getElementById('results').innerHTML = `<div class="error">Error: ${error.message}<br>Please use format like: QS 8D 8S AC 2H</div>`;
            }
        }

        function analyzeHand() {
            if (currentHand.length !== 5) {
                document.getElementById('results').innerHTML = '<div class="error">Please enter a complete 5-card hand first.</div>';
                return;
            }

            document.getElementById('results').innerHTML = '<div class="loading">Analyzing hand strategies...</div>';

            // Use setTimeout to allow UI to update
            setTimeout(() => {
                try {
                    const strategies = analyzer.analyzeHand(currentHand);
                    displayResults(strategies);
                } catch (error) {
                    document.getElementById('results').innerHTML = `<div class="error">Analysis error: ${error.message}</div>`;
                }
            }, 100);
        }

        function displayResults(strategies) {
            const resultsDiv = document.getElementById('results');
            let html = '';

            strategies.forEach((strategy, index) => {
                const isBest = index === 0;
                const heldCardsStr = strategy.heldCards.length > 0 
                    ? strategy.heldCards.map(card => card.toString()).join(' ')
                    : 'None (Draw 5)';
                
                const positionStr = strategy.holdIndices.map(i => i + 1).join(', ');

                html += `
                    <div class="strategy-item ${isBest ? 'best' : ''}">
                        <div class="strategy-rank">#${index + 1}</div>
                        <div class="strategy-hold">Hold: ${heldCardsStr} ${positionStr ? `[${positionStr}]` : ''}</div>
                        <div class="strategy-ev">Expected Value: ${strategy.expectedValue.toFixed(4)}</div>
                `;

                if (index < 5 && Object.keys(strategy.outcomes).length > 0) {
                    const totalOutcomes = Object.values(strategy.outcomes).reduce((sum, count) => sum + count, 0);
                    const outcomeLines = [];
                    
                    const sortedOutcomes = Object.entries(strategy.outcomes)
                        .filter(([_, count]) => count > 0)
                        .sort(([typeA], [typeB]) => (HandEvaluator.PAYOUTS[typeB] || 0) - (HandEvaluator.PAYOUTS[typeA] || 0));

                    for (const [handType, count] of sortedOutcomes.slice(0, 4)) {
                        const percentage = ((count / totalOutcomes) * 100).toFixed(1);
                        const payout = HandEvaluator.PAYOUTS[handType] || 0;
                        if (payout > 0) {
                            outcomeLines.push(`${handType}: ${percentage}% (${payout}x)`);
                        }
                    }

                    if (outcomeLines.length > 0) {
                        html += `<div class="strategy-outcomes">Outcomes: ${outcomeLines.join(', ')}</div>`;
                    }
                }

                html += '</div>';
            });

            if (strategies.length > 0) {
                const best = strategies[0];
                const recommendationText = best.heldCards.length > 0
                    ? `Hold: ${best.heldCards.map(card => card.toString()).join(' ')}`
                    : 'Hold: None (Draw 5 new cards)';

                html += `
                    <div class="recommendation">
                        <h3>🎯 OPTIMAL STRATEGY</h3>
                        <div class="recommendation-text">${recommendationText}</div>
                        <div style="margin-top: 10px; color: #fbbf24;">Expected Value: ${best.expectedValue.toFixed(4)}</div>
                    </div>
                `;
            }

            resultsDiv.innerHTML = html;
        }

        // Initialize with a random hand
        window.onload = function() {
            generateRandomHand();
        };
    </script>
</body>
</html>
