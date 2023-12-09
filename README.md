import 'package:flutter/material.dart';
import 'dart:ui';

void main() {
  runApp(QuizApp());
}

class QuizApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: QuizScreen(),
    );
  }
}

class QuizScreen extends StatefulWidget {
  @override
  _QuizScreenState createState() => _QuizScreenState();
}

class _QuizScreenState extends State<QuizScreen> {
  int _questionIndex = 0;
  int _score = 0;

  List<Map<String, Object>> _questions = [
    {
      'question': 'What is the capital of Italy?',
      'answers': [
        {'text': 'Berlin', 'score': 0},
        {'text': 'Paris', 'score': 0},
        {'text': 'Madrid', 'score': 0},
        {'text': 'Rome', 'score': 10},
      ],
    },
    {
      'question': 'Which programming language is Flutter based on?',
      'answers': [
        {'text': 'Java', 'score': 0},
        {'text': 'Dart', 'score': 10},
        {'text': 'C++', 'score': 0},
        {'text': 'Python', 'score': 0},
      ],
    },
    // Add more questions as needed
  ];

  void _answerQuestion(int score) {
    setState(() {
      _score += score;

      if (_questionIndex < _questions.length - 1) {
        _questionIndex++;
      } else {
        // End of the quiz
        _showResultDialog();
      }
    });
  }

  void _resetQuiz() {
    setState(() {
      _questionIndex = 0;
      _score = 0;
    });
  }

  void _showResultDialog() {
    showDialog(
      context: context,
      builder: (BuildContext context) {
        return AlertDialog(
          title: Text('Quiz Completed'),
          content: Text('Your Score: $_score/${_questions.length * 10}'),
          actions: [
            TextButton(
              onPressed: () {
                Navigator.of(context).pop();
                _resetQuiz();
              },
              child: Text('Restart Quiz'),
            ),
          ],
        );
      },
    );
  }

  @override
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Quiz Time'),
      ),
      backgroundColor: const Color.fromARGB(
          255, 9, 89, 128), // Set your desired background color
      body: _questionIndex < _questions.length
          ? QuizQuestion(
              question: _questions[_questionIndex]['question'] as String,
              answers: (_questions[_questionIndex]['answers']
                  as List<Map<String, Object>>),
              answerCallback: _answerQuestion,
            )
          : Center(
              child: Text('Quiz Completed!'),
            ),
    );
  }
}

class QuizQuestion extends StatelessWidget {
  final String question;
  final List<Map<String, Object>> answers;
  final Function(int) answerCallback;

  QuizQuestion({
    required this.question,
    required this.answers,
    required this.answerCallback,
  });

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Padding(
          padding: const EdgeInsets.all(16.0),
          child: Text(
            question,
            style: TextStyle(fontSize: 22),
          ),
        ),
        ...answers.map((answer) {
          return Padding(
            padding:
                const EdgeInsets.symmetric(horizontal: 16.0, vertical: 8.0),
            child: ElevatedButton(
              onPressed: () => answerCallback(answer['score'] as int),
              child: Text(
                  answer['text'] as String), // Use 'text' instead of 'score'
            ),
          );
        }).toList(),
      ],
    );
  }
}
