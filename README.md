import React, { useState, useEffect } from 'react';

// Main App component
function App() {
  // State variables for the game
  const [score, setScore] = useState(0); // Player's current score
  const [question, setQuestion] = useState(''); // The math question string
  const [answer, setAnswer] = useState(''); // The correct answer to the current question
  const [userInput, setUserInput] = useState(''); // User's typed input
  const [feedback, setFeedback] = useState(''); // Feedback message (Correct/Incorrect)
  const [gameActive, setGameActive] = useState(false); // Game active status
  const [questionCount, setQuestionCount] = useState(0); // Number of questions asked
  const maxQuestions = 10; // Total questions in a game

  // Function to generate a random math problem
  const generateQuestion = () => {
    const num1 = Math.floor(Math.random() * 10) + 1; // Random number between 1 and 10
    const num2 = Math.floor(Math.random() * 10) + 1; // Random number between 1 and 10
    const operations = ['+', '-', '*']; // Available operations
    const operation = operations[Math.floor(Math.random() * operations.length)]; // Random operation

    let newQuestion = '';
    let correctAnswer = 0;

    // Generate question and calculate correct answer based on operation
    switch (operation) {
      case '+':
        newQuestion = `${num1} + ${num2} = ?`;
        correctAnswer = num1 + num2;
        break;
      case '-':
        // Ensure the result is not negative for subtraction
        if (num1 < num2) {
          newQuestion = `${num2} - ${num1} = ?`;
          correctAnswer = num2 - num1;
        } else {
          newQuestion = `${num1} - ${num2} = ?`;
          correctAnswer = num1 - num2;
        }
        break;
      case '*':
        newQuestion = `${num1} * ${num2} = ?`;
        correctAnswer = num1 * num2;
        break;
      default:
        break;
    }

    setQuestion(newQuestion);
    setAnswer(correctAnswer.toString()); // Store answer as string for comparison with user input
    setUserInput(''); // Clear user input for the new question
    setFeedback(''); // Clear feedback for the new question
    setQuestionCount(prevCount => prevCount + 1); // Increment question count
  };

  // Function to start the game
  const startGame = () => {
    setScore(0); // Reset score
    setQuestionCount(0); // Reset question count
    setGameActive(true); // Set game to active
    generateQuestion(); // Generate the first question
  };

  // Function to handle user's answer submission
  const handleSubmit = () => {
    // Check if user input is empty
    if (userInput.trim() === '') {
      setFeedback('Please enter an answer!');
      return;
    }

    // Compare user's answer with the correct answer
    if (userInput === answer) {
      setFeedback('Correct! 🎉');
      setScore(prevScore => prevScore + 1); // Increment score for correct answer
    } else {
      setFeedback(`Incorrect. The answer was ${answer}. 😔`);
    }

    // Move to the next question after a short delay for feedback
    setTimeout(() => {
      if (questionCount < maxQuestions) {
        generateQuestion();
      } else {
        setGameActive(false); // End game if max questions reached
      }
    }, 1500); // 1.5 second delay
  };

  // Handle Enter key press for submission
  const handleKeyPress = (event) => {
    if (event.key === 'Enter') {
      handleSubmit();
    }
  };

  // Render the game UI
  return (
    <div className="min-h-screen bg-gradient-to-br from-purple-400 to-pink-500 flex items-center justify-center p-4">
      <div className="bg-white rounded-3xl shadow-2xl p-8 max-w-md w-full text-center transform transition-transform duration-500 hover:scale-105">
        <h1 className="text-4xl font-extrabold text-gray-800 mb-6 font-inter">
          Math Whiz! 🧠
        </h1>

        {!gameActive ? (
          // Start screen
          <div className="space-y-6">
            <p className="text-xl text-gray-700 font-inter">
              Ready to test your math skills?
            </p>
            <button
              onClick={startGame}
              className="w-full bg-green-500 hover:bg-green-600 text-white font-bold py-4 px-6 rounded-xl shadow-lg transform transition-all duration-300 hover:scale-105 focus:outline-none focus:ring-4 focus:ring-green-300"
            >
              Start Game!
            </button>
            {questionCount === maxQuestions && ( // Show final score if game ended
              <p className="text-2xl font-bold text-blue-700 mt-4 font-inter">
                Game Over! Your score: {score} / {maxQuestions}
              </p>
            )}
          </div>
        ) : (
          // Game in progress
          <div className="space-y-6">
            <div className="flex justify-between items-center mb-4">
              <p className="text-lg font-semibold text-gray-600 font-inter">
                Score: <span className="text-blue-600 text-2xl">{score}</span>
              </p>
              <p className="text-lg font-semibold text-gray-600 font-inter">
                Question: <span className="text-blue-600 text-2xl">{questionCount} / {maxQuestions}</span>
              </p>
            </div>

            <p className="text-5xl font-bold text-purple-700 mb-8 font-inter">
              {question}
            </p>

            <input
              type="number"
              value={userInput}
              onChange={(e) => setUserInput(e.target.value)}
              onKeyPress={handleKeyPress}
              placeholder="Your Answer"
              className="w-full p-4 text-3xl text-center border-4 border-blue-300 rounded-xl focus:outline-none focus:border-blue-500 transition-all duration-300 shadow-md font-inter"
            />

            <p className="text-2xl font-semibold mt-4 h-8 font-inter" style={{ color: feedback.includes('Correct') ? '#22C55E' : '#EF4444' }}>
              {feedback}
            </p>

            <button
              onClick={handleSubmit}
              className="w-full bg-blue-500 hover:bg-blue-600 text-white font-bold py-4 px-6 rounded-xl shadow-lg transform transition-all duration-300 hover:scale-105 focus:outline-none focus:ring-4 focus:ring-blue-300"
            >
              Submit Answer
            </button>
          </div>
        )}
      </div>
    </div>
  );
}

export default App;
