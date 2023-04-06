## [Tutorial: Tic-Tac-Toe](https://react.dev/learn/tutorial-tic-tac-toe)
- You will build a small tic-tac-toe game during this tutorial.
    - This tutorial does not assume any existing React knowledge.
    - The techniques you’ll learn in the tutorial are fundamental to building any React app, and fully understanding it will give you a deep understanding of React.
- The tutorial is divided into several sections:
    - [Setup for the tutorial](#setup-for-the-tutorial) will give you a starting point to follow the tutorial.
    - [Overview](#overview) will teach you the fundamentals of React: components, props, and state.
    - [Completing the game](#completing-the-game) will teach you the most common techniques in React development.
    - [Adding time travel](#adding-time-travel) will give you a deeper insight into the unique strengths of React.

### Table of Contents
1. [What are you building?](#what-are-you-building)

### What are you building?
- In this tutorial, you’ll build an interactive tic-tac-toe game with React.
- You can see what it will look like when you’re finished here:
    ```js
    //App.js
    import { useState } from 'react';

    function Square({ value, onSquareClick }) {
        return (
            <button className="square" onClick={onSquareClick}>
                {value}
            </button>
        );
    }

    function Board({ xIsNext, squares, onPlay }) {
        function handleClick(i) {
            if (calculateWinner(squares) || squares[i]) {
                return;
            }
            const nextSquares = squares.slice();
            if (xIsNext) {
                nextSquares[i] = 'X';
            } else {
                nextSquares[i] = 'O';
            }
            onPlay(nextSquares);
        }

        const winner = calculateWinner(squares);
        let status;
        if (winner) {
            status = 'Winner: ' + winner;
        } else {
            status = 'Next player: ' + (xIsNext ? 'X' : 'O');
        }

        return (
            <>
                <div className="status">{status}</div>
                <div className="board-row">
                    <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
                    <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
                    <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
                </div>
                <div className="board-row">
                    <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
                    <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
                    <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
                </div>
                <div className="board-row">
                    <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
                    <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
                    <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
                </div>
            </>
        );
    }

    export default function Game() {
        const [history, setHistory] = useState([Array(9).fill(null)]);
        const [currentMove, setCurrentMove] = useState(0);
        const xIsNext = currentMove % 2 === 0;
        const currentSquares = history[currentMove];

        function handlePlay(nextSquares) {
            const nextHistory = [...history.slice(0, currentMove + 1), nextSquares];
            setHistory(nextHistory);
            setCurrentMove(nextHistory.length - 1);
        }

        function jumpTo(nextMove) {
            setCurrentMove(nextMove);
        }

        const moves = history.map((squares, move) => {
            let description;
            if (move > 0) {
                description = 'Go to move #' + move;
            } else {
                description = 'Go to game start';
            }
            return (
                <li key={move}>
                    <button onClick={() => jumpTo(move)}>{description}</button>
                </li>
            );
        });

        return (
            <div className="game">
            <div className="game-board">
                <Board xIsNext={xIsNext} squares={currentSquares} onPlay={handlePlay} />
            </div>
            <div className="game-info">
                <ol>{moves}</ol>
            </div>
            </div>
        );
    }

    function calculateWinner(squares) {
        const lines = [
            [0, 1, 2],
            [3, 4, 5],
            [6, 7, 8],
            [0, 3, 6],
            [1, 4, 7],
            [2, 5, 8],
            [0, 4, 8],
            [2, 4, 6],
        ];
        for (let i = 0; i < lines.length; i++) {
            const [a, b, c] = lines[i];
            if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
            return squares[a];
            }
        }
        return null;
    }
    ```
- If the code doesn’t make sense to you yet, or if you are unfamiliar with the code’s syntax, don’t worry!
    - The goal of this tutorial is to help you understand React and its syntax.
- We recommend that you check out the tic-tac-toe game above before continuing with the tutorial.
    - One of the features that you’ll notice is that there is a numbered list to the right of the game’s board.
    - This list gives you a history of all of the moves that have occurred in the game, and it is updated as the game progresses.
- Once you’ve played around with the finished tic-tac-toe game, keep scrolling.
    - You’ll start with a simpler template in this tutorial. Our next step is to set you up so that you can start building the game.