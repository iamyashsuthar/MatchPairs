import 'dart:async';
import 'dart:math';
import 'package:flutter/material.dart';

void main() {
  runApp(const PurblePairsApp());
}

// ─── App Root ────────────────────────────────────────────────────────────────

class PurblePairsApp extends StatelessWidget {
  const PurblePairsApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Purble Pairs',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        fontFamily: 'Segoe UI',
        colorScheme: ColorScheme.fromSeed(seedColor: const Color(0xFF5B9BD5)),
        useMaterial3: true,
      ),
      home: const MenuScreen(),
    );
  }
}

// ─── Difficulty Config ────────────────────────────────────────────────────────

enum Difficulty { beginner, intermediate, advanced }

class DifficultyConfig {
  final String label;
  final int cols;
  final int rows;
  final Color color;

  const DifficultyConfig({
    required this.label,
    required this.cols,
    required this.rows,
    required this.color,
  });

  int get totalCards => cols * rows;
  int get pairs => totalCards ~/ 2;
}

const Map<Difficulty, DifficultyConfig> kDifficultyMap = {
  Difficulty.beginner: DifficultyConfig(
    label: 'Beginner',
    cols: 4,
    rows: 3,
    color: Color(0xFF4CAF50),
  ),
  Difficulty.intermediate: DifficultyConfig(
    label: 'Intermediate',
    cols: 4,
    rows: 5,
    color: Color(0xFF2196F3),
  ),
  Difficulty.advanced: DifficultyConfig(
    label: 'Advanced',
    cols: 6,
    rows: 6,
    color: Color(0xFFF44336),
  ),
};

// ─── Emoji Sets ───────────────────────────────────────────────────────────────

const List<String> kAllEmojis = [
  '🐶', '🐱', '🐭', '🐹', '🐰', '🦊', '🐻', '🐼',
  '🐨', '🐯', '🦁', '🐸', '🐵', '🐔', '🐧', '🐦',
  '🦆', '🦅', '🦉', '🦇', '🐝', '🐛', '🦋', '🐌',
  '🐞', '🐜', '🦟', '🦗', '🕷', '🦂', '🐢', '🐍',
  '🦎', '🐊', '🐙', '🦑', '🦞', '🦀', '🐡', '🐠',
  '🐟', '🐬', '🐳', '🦈', '🐅', '🐆', '🦓', '🦍',
  '🦧', '🦣', '🐘', '🦛', '🦏', '🐪', '🦒', '🦘',
  '🦬', '🐃', '🐂', '🐄', '🐎', '🐖', '🐏', '🐑',
];

// ─── Card Model ───────────────────────────────────────────────────────────────

class CardModel {
  final int id;
  final String emoji;
  bool isFaceUp;
  bool isMatched;

  CardModel({
    required this.id,
    required this.emoji,
    this.isFaceUp = false,
    this.isMatched = false,
  });
}

// ─── Menu Screen ─────────────────────────────────────────────────────────────

class MenuScreen extends StatelessWidget {
  const MenuScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Container(
        decoration: const BoxDecoration(
          gradient: LinearGradient(
            begin: Alignment.topLeft,
            end: Alignment.bottomRight,
            colors: [Color(0xFF1a2a6c), Color(0xFF2980b9), Color(0xFF6dd5fa)],
          ),
        ),
        child: SafeArea(
          child: Center(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                // Title card
                Container(
                  margin: const EdgeInsets.symmetric(horizontal: 32),
                  padding: const EdgeInsets.symmetric(vertical: 24, horizontal: 32),
                  decoration: BoxDecoration(
                    color: Colors.white.withOpacity(0.15),
                    borderRadius: BorderRadius.circular(24),
                    border: Border.all(color: Colors.white.withOpacity(0.4), width: 2),
                    boxShadow: [
                      BoxShadow(
                        color: Colors.black.withOpacity(0.3),
                        blurRadius: 20,
                        offset: const Offset(0, 8),
                      ),
                    ],
                  ),
                  child: Column(
                    children: [
                      const Text(
                        '🃏',
                        style: TextStyle(fontSize: 64),
                      ),
                      const SizedBox(height: 8),
                      const Text(
                        'PURBLE PAIRS',
                        style: TextStyle(
                          fontSize: 32,
                          fontWeight: FontWeight.w900,
                          color: Colors.white,
                          letterSpacing: 4,
                          shadows: [
                            Shadow(color: Colors.black45, blurRadius: 8, offset: Offset(2, 2)),
                          ],
                        ),
                      ),
                      const SizedBox(height: 4),
                      Text(
                        'Memory Matching Game',
                        style: TextStyle(
                          fontSize: 14,
                          color: Colors.white.withOpacity(0.8),
                          letterSpacing: 2,
                        ),
                      ),
                    ],
                  ),
                ),

                const SizedBox(height: 48),

                // Difficulty buttons
                const Text(
                  'SELECT DIFFICULTY',
                  style: TextStyle(
                    color: Colors.white70,
                    fontSize: 13,
                    letterSpacing: 3,
                    fontWeight: FontWeight.w600,
                  ),
                ),
                const SizedBox(height: 16),

                for (final entry in kDifficultyMap.entries)
                  _DifficultyButton(
                    difficulty: entry.key,
                    config: entry.value,
                    onTap: () => Navigator.of(context).push(
                      MaterialPageRoute(
                        builder: (_) => GameScreen(
                          difficulty: entry.key,
                          config: entry.value,
                        ),
                      ),
                    ),
                  ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}

class _DifficultyButton extends StatefulWidget {
  final Difficulty difficulty;
  final DifficultyConfig config;
  final VoidCallback onTap;

  const _DifficultyButton({
    required this.difficulty,
    required this.config,
    required this.onTap,
  });

  @override
  State<_DifficultyButton> createState() => _DifficultyButtonState();
}

class _DifficultyButtonState extends State<_DifficultyButton> {
  bool _pressed = false;

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTapDown: (_) => setState(() => _pressed = true),
      onTapUp: (_) {
        setState(() => _pressed = false);
        widget.onTap();
      },
      onTapCancel: () => setState(() => _pressed = false),
      child: AnimatedScale(
        scale: _pressed ? 0.96 : 1.0,
        duration: const Duration(milliseconds: 80),
        child: Container(
          margin: const EdgeInsets.symmetric(horizontal: 48, vertical: 8),
          padding: const EdgeInsets.symmetric(vertical: 16),
          decoration: BoxDecoration(
            color: widget.config.color,
            borderRadius: BorderRadius.circular(16),
            boxShadow: [
              BoxShadow(
                color: widget.config.color.withOpacity(0.5),
                blurRadius: 12,
                offset: const Offset(0, 4),
              ),
            ],
          ),
          child: Row(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Text(
                widget.config.label,
                style: const TextStyle(
                  color: Colors.white,
                  fontSize: 18,
                  fontWeight: FontWeight.w700,
                  letterSpacing: 1,
                ),
              ),
              const SizedBox(width: 12),
              Text(
                '${widget.config.cols}×${widget.config.rows}',
                style: TextStyle(
                  color: Colors.white.withOpacity(0.75),
                  fontSize: 14,
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

// ─── Game Screen ──────────────────────────────────────────────────────────────

class GameScreen extends StatefulWidget {
  final Difficulty difficulty;
  final DifficultyConfig config;

  const GameScreen({super.key, required this.difficulty, required this.config});

  @override
  State<GameScreen> createState() => _GameScreenState();
}

class _GameScreenState extends State<GameScreen> with TickerProviderStateMixin {
  late List<CardModel> _cards;
  final List<int> _flipped = [];
  bool _locked = false;
  int _moves = 0;
  int _matches = 0;
  late Stopwatch _stopwatch;
  Timer? _timer;
  String _elapsed = '00:00';
  bool _gameOver = false;

  // Win animation controller
  late AnimationController _winController;
  late Animation<double> _winScale;

  @override
  void initState() {
    super.initState();
    _winController = AnimationController(
      vsync: this,
      duration: const Duration(milliseconds: 600),
    );
    _winScale = CurvedAnimation(parent: _winController, curve: Curves.elasticOut);
    _initGame();
  }

  void _initGame() {
    final random = Random();
    final emojis = [...kAllEmojis]..shuffle(random);
    final selected = emojis.take(widget.config.pairs).toList();
    final doubled = [...selected, ...selected]..shuffle(random);

    _cards = List.generate(
      doubled.length,
      (i) => CardModel(id: i, emoji: doubled[i]),
    );

    _flipped.clear();
    _locked = false;
    _moves = 0;
    _matches = 0;
    _gameOver = false;
    _stopwatch = Stopwatch()..start();

    _timer?.cancel();
    _timer = Timer.periodic(const Duration(seconds: 1), (_) {
      if (mounted) {
        setState(() {
          final s = _stopwatch.elapsed.inSeconds;
          final m = s ~/ 60;
          final sec = s % 60;
          _elapsed = '${m.toString().padLeft(2, '0')}:${sec.toString().padLeft(2, '0')}';
        });
      }
    });
  }

  void _onCardTap(int index) {
    if (_locked) return;
    if (_cards[index].isMatched) return;
    if (_cards[index].isFaceUp) return;

    setState(() {
      _cards[index].isFaceUp = true;
      _flipped.add(index);
    });

    if (_flipped.length == 2) {
      _locked = true;
      _moves++;
      _checkMatch();
    }
  }

  void _checkMatch() {
    final a = _flipped[0];
    final b = _flipped[1];

    if (_cards[a].emoji == _cards[b].emoji) {
      // Match!
      Future.delayed(const Duration(milliseconds: 400), () {
        if (!mounted) return;
        setState(() {
          _cards[a].isMatched = true;
          _cards[b].isMatched = true;
          _flipped.clear();
          _locked = false;
          _matches++;
        });
        if (_matches == widget.config.pairs) {
          _stopwatch.stop();
          _timer?.cancel();
          setState(() => _gameOver = true);
          _winController.forward(from: 0);
        }
      });
    } else {
      // No match — flip back after delay (Purble Pairs style: brief peek)
      Future.delayed(const Duration(milliseconds: 900), () {
        if (!mounted) return;
        setState(() {
          _cards[a].isFaceUp = false;
          _cards[b].isFaceUp = false;
          _flipped.clear();
          _locked = false;
        });
      });
    }
  }

  void _restartGame() {
    _winController.reset();
    setState(() {
      _initGame();
    });
  }

  @override
  void dispose() {
    _timer?.cancel();
    _winController.dispose();
    super.dispose();
  }

  String get _scoreRating {
    final optimal = widget.config.pairs;
    if (_moves <= optimal + 2) return '⭐⭐⭐ Perfect!';
    if (_moves <= optimal * 2) return '⭐⭐ Great!';
    if (_moves <= optimal * 3) return '⭐ Good!';
    return '🏁 Finished!';
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Container(
        decoration: const BoxDecoration(
          gradient: LinearGradient(
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
            colors: [Color(0xFF1a2a6c), Color(0xFF2980b9)],
          ),
        ),
        child: SafeArea(
          child: Stack(
            children: [
              Column(
                children: [
                  _buildTopBar(context),
                  Expanded(child: _buildGrid()),
                  _buildBottomBar(),
                ],
              ),
              if (_gameOver) _buildWinOverlay(),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildTopBar(BuildContext context) {
    return Container(
      padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 10),
      child: Row(
        children: [
          // Back button
          GestureDetector(
            onTap: () => Navigator.of(context).pop(),
            child: Container(
              padding: const EdgeInsets.all(8),
              decoration: BoxDecoration(
                color: Colors.white.withOpacity(0.15),
                borderRadius: BorderRadius.circular(10),
              ),
              child: const Icon(Icons.arrow_back, color: Colors.white, size: 20),
            ),
          ),
          const SizedBox(width: 12),
          // Title
          Expanded(
            child: Text(
              widget.config.label.toUpperCase(),
              style: const TextStyle(
                color: Colors.white,
                fontSize: 16,
                fontWeight: FontWeight.w800,
                letterSpacing: 2,
              ),
            ),
          ),
          // Stats
          _StatChip(icon: '🔄', value: '$_moves', label: 'moves'),
          const SizedBox(width: 8),
          _StatChip(icon: '✅', value: '$_matches', label: 'pairs'),
          const SizedBox(width: 8),
          _StatChip(icon: '⏱', value: _elapsed, label: ''),
        ],
      ),
    );
  }

  Widget _buildGrid() {
    return Padding(
      padding: const EdgeInsets.symmetric(horizontal: 10, vertical: 4),
      child: LayoutBuilder(
        builder: (context, constraints) {
          final cols = widget.config.cols;
          final rows = widget.config.rows;
          final spacing = 6.0;
          final cardW = (constraints.maxWidth - spacing * (cols - 1)) / cols;
          final cardH = (constraints.maxHeight - spacing * (rows - 1)) / rows;
          final cardSize = min(cardW, cardH);

          return Center(
            child: Wrap(
              spacing: spacing,
              runSpacing: spacing,
              alignment: WrapAlignment.center,
              children: List.generate(_cards.length, (i) {
                return SizedBox(
                  width: cardSize,
                  height: cardSize,
                  child: _MemoryCard(
                    card: _cards[i],
                    size: cardSize,
                    onTap: () => _onCardTap(i),
                  ),
                );
              }),
            ),
          );
        },
      ),
    );
  }

  Widget _buildBottomBar() {
    return Container(
      padding: const EdgeInsets.all(16),
      child: GestureDetector(
        onTap: _restartGame,
        child: Container(
          padding: const EdgeInsets.symmetric(vertical: 14, horizontal: 48),
          decoration: BoxDecoration(
            color: const Color(0xFFe74c3c),
            borderRadius: BorderRadius.circular(14),
            boxShadow: [
              BoxShadow(
                color: const Color(0xFFe74c3c).withOpacity(0.4),
                blurRadius: 10,
                offset: const Offset(0, 4),
              ),
            ],
          ),
          child: const Row(
            mainAxisSize: MainAxisSize.min,
            children: [
              Icon(Icons.refresh, color: Colors.white, size: 20),
              SizedBox(width: 8),
              Text(
                'RESTART',
                style: TextStyle(
                  color: Colors.white,
                  fontWeight: FontWeight.w800,
                  fontSize: 16,
                  letterSpacing: 2,
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildWinOverlay() {
    return Container(
      color: Colors.black.withOpacity(0.65),
      child: Center(
        child: ScaleTransition(
          scale: _winScale,
          child: Container(
            margin: const EdgeInsets.symmetric(horizontal: 32),
            padding: const EdgeInsets.all(32),
            decoration: BoxDecoration(
              color: Colors.white,
              borderRadius: BorderRadius.circular(28),
              boxShadow: [
                BoxShadow(
                  color: Colors.black.withOpacity(0.4),
                  blurRadius: 30,
                  offset: const Offset(0, 10),
                ),
              ],
            ),
            child: Column(
              mainAxisSize: MainAxisSize.min,
              children: [
                const Text('🎉', style: TextStyle(fontSize: 64)),
                const SizedBox(height: 8),
                const Text(
                  'YOU WIN!',
                  style: TextStyle(
                    fontSize: 32,
                    fontWeight: FontWeight.w900,
                    color: Color(0xFF1a2a6c),
                    letterSpacing: 3,
                  ),
                ),
                const SizedBox(height: 16),
                Text(
                  _scoreRating,
                  style: const TextStyle(fontSize: 22, color: Color(0xFF2980b9)),
                ),
                const SizedBox(height: 12),
                _WinStat(label: 'Moves', value: '$_moves'),
                _WinStat(label: 'Time', value: _elapsed),
                _WinStat(label: 'Pairs', value: '${widget.config.pairs}'),
                const SizedBox(height: 24),
                Row(
                  children: [
                    Expanded(
                      child: _WinButton(
                        label: '🔄 Play Again',
                        color: const Color(0xFF2ecc71),
                        onTap: _restartGame,
                      ),
                    ),
                    const SizedBox(width: 12),
                    Expanded(
                      child: _WinButton(
                        label: '🏠 Menu',
                        color: const Color(0xFF2980b9),
                        onTap: () => Navigator.of(context).pop(),
                      ),
                    ),
                  ],
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}

// ─── Memory Card Widget ───────────────────────────────────────────────────────

class _MemoryCard extends StatefulWidget {
  final CardModel card;
  final double size;
  final VoidCallback onTap;

  const _MemoryCard({
    required this.card,
    required this.size,
    required this.onTap,
  });

  @override
  State<_MemoryCard> createState() => _MemoryCardState();
}

class _MemoryCardState extends State<_MemoryCard>
    with SingleTickerProviderStateMixin {
  late AnimationController _flipController;
  late Animation<double> _flipAnim;
  bool _showFront = false;

  // Purble Pairs card back colors cycling list
  static const List<Color> _backColors = [
    Color(0xFF3498db),
    Color(0xFF9b59b6),
    Color(0xFF1abc9c),
    Color(0xFFe67e22),
    Color(0xFFe74c3c),
    Color(0xFF2ecc71),
  ];

  @override
  void initState() {
    super.initState();
    _flipController = AnimationController(
      vsync: this,
      duration: const Duration(milliseconds: 350),
    );
    _flipAnim = Tween<double>(begin: 0, end: 1).animate(
      CurvedAnimation(parent: _flipController, curve: Curves.easeInOut),
    );
    _showFront = widget.card.isFaceUp || widget.card.isMatched;
    if (_showFront) _flipController.value = 1.0;
  }

  @override
  void didUpdateWidget(_MemoryCard old) {
    super.didUpdateWidget(old);
    final shouldShow = widget.card.isFaceUp || widget.card.isMatched;
    if (shouldShow != _showFront) {
      _showFront = shouldShow;
      if (_showFront) {
        _flipController.forward();
      } else {
        _flipController.reverse();
      }
    }
  }

  @override
  void dispose() {
    _flipController.dispose();
    super.dispose();
  }

  Color get _backColor {
    return _backColors[widget.card.id % _backColors.length];
  }

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: widget.card.isMatched ? null : widget.onTap,
      child: AnimatedBuilder(
        animation: _flipAnim,
        builder: (context, child) {
          final isShowingFront = _flipAnim.value >= 0.5;
          final angle = isShowingFront
              ? (_flipAnim.value - 1) * pi
              : _flipAnim.value * pi;

          return Transform(
            alignment: Alignment.center,
            transform: Matrix4.identity()
              ..setEntry(3, 2, 0.001)
              ..rotateY(angle),
            child: isShowingFront
                ? _buildFront()
                : _buildBack(),
          );
        },
      ),
    );
  }

  Widget _buildBack() {
    return Container(
      decoration: BoxDecoration(
        color: _backColor,
        borderRadius: BorderRadius.circular(widget.size * 0.14),
        boxShadow: [
          BoxShadow(
            color: _backColor.withOpacity(0.5),
            blurRadius: 6,
            offset: const Offset(0, 3),
          ),
        ],
      ),
      child: Center(
        child: Text(
          '?',
          style: TextStyle(
            fontSize: widget.size * 0.45,
            fontWeight: FontWeight.w900,
            color: Colors.white.withOpacity(0.9),
            shadows: const [
              Shadow(color: Colors.black38, blurRadius: 4, offset: Offset(1, 2)),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildFront() {
    return AnimatedContainer(
      duration: const Duration(milliseconds: 200),
      decoration: BoxDecoration(
        color: widget.card.isMatched
            ? const Color(0xFFd5f5e3)
            : Colors.white,
        borderRadius: BorderRadius.circular(widget.size * 0.14),
        border: Border.all(
          color: widget.card.isMatched
              ? const Color(0xFF2ecc71)
              : Colors.grey.shade200,
          width: 2,
        ),
        boxShadow: [
          BoxShadow(
            color: widget.card.isMatched
                ? const Color(0xFF2ecc71).withOpacity(0.4)
                : Colors.black.withOpacity(0.2),
            blurRadius: widget.card.isMatched ? 10 : 4,
            offset: const Offset(0, 2),
          ),
        ],
      ),
      child: Center(
        child: Text(
          widget.card.emoji,
          style: TextStyle(fontSize: widget.size * 0.5),
        ),
      ),
    );
  }
}

// ─── Small Reusable Widgets ───────────────────────────────────────────────────

class _StatChip extends StatelessWidget {
  final String icon;
  final String value;
  final String label;

  const _StatChip({
    required this.icon,
    required this.value,
    required this.label,
  });

  @override
  Widget build(BuildContext context) {
    return Container(
      padding: const EdgeInsets.symmetric(horizontal: 10, vertical: 6),
      decoration: BoxDecoration(
        color: Colors.white.withOpacity(0.15),
        borderRadius: BorderRadius.circular(10),
      ),
      child: Row(
        mainAxisSize: MainAxisSize.min,
        children: [
          Text(icon, style: const TextStyle(fontSize: 12)),
          const SizedBox(width: 4),
          Text(
            value,
            style: const TextStyle(
              color: Colors.white,
              fontWeight: FontWeight.w700,
              fontSize: 13,
            ),
          ),
          if (label.isNotEmpty) ...[
            const SizedBox(width: 2),
            Text(
              label,
              style: TextStyle(
                color: Colors.white.withOpacity(0.6),
                fontSize: 10,
              ),
            ),
          ],
        ],
      ),
    );
  }
}

class _WinStat extends StatelessWidget {
  final String label;
  final String value;

  const _WinStat({required this.label, required this.value});

  @override
  Widget build(BuildContext context) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 4),
      child: Row(
        mainAxisAlignment: MainAxisAlignment.spaceBetween,
        children: [
          Text(
            label,
            style: const TextStyle(
              color: Colors.grey,
              fontSize: 15,
            ),
          ),
          Text(
            value,
            style: const TextStyle(
              color: Color(0xFF1a2a6c),
              fontWeight: FontWeight.w700,
              fontSize: 15,
            ),
          ),
        ],
      ),
    );
  }
}

class _WinButton extends StatelessWidget {
  final String label;
  final Color color;
  final VoidCallback onTap;

  const _WinButton({
    required this.label,
    required this.color,
    required this.onTap,
  });

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: onTap,
      child: Container(
        padding: const EdgeInsets.symmetric(vertical: 14),
        decoration: BoxDecoration(
          color: color,
          borderRadius: BorderRadius.circular(14),
          boxShadow: [
            BoxShadow(
              color: color.withOpacity(0.4),
              blurRadius: 8,
              offset: const Offset(0, 4),
            ),
          ],
        ),
        child: Center(
          child: Text(
            label,
            style: const TextStyle(
              color: Colors.white,
              fontWeight: FontWeight.w700,
              fontSize: 15,
            ),
          ),
        ),
      ),
    );
  }
}
