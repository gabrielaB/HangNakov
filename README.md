using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.IO;

namespace Hanger
{
    class Program
    {
        private static int lives = 8;
        static void Main()
        {
            Random random = new Random((int)DateTime.Now.Ticks);
            //Open a file from the folder and select a random word from it.
            string[] wordBank = File.ReadAllLines(@"..\..\Level1.txt");
            string GuessedWord = wordBank[random.Next(0, wordBank.Length)];

            string GuessedWordUpperCase = GuessedWord.ToUpper();
            //Take the length of the word and replace each symbol with "_"
            StringBuilder DisplayWord = new StringBuilder(GuessedWord.Length);
            DrawWord(GuessedWord, DisplayWord);
            List<char> GuessedLetters = new List<char>();
            List<char> IncorrectLetters = new List<char>();


            bool win = false;
            int revealedletters = 0;
            int level = 1;

            string input;
            char guess = ' ';
            // Game starts from Level 1
            while (lives > 0 && level == 1)
            {
                Console.Write("Guess a letter: ");

                input = Console.ReadLine().ToUpper();
                if (!string.IsNullOrWhiteSpace(input))
                {
                    guess = input[0];
                }
                else
                {
                    guess = ' ';
                }
                //Indicator that the user has already entered this letter.
                if (GuessedLetters.Contains(guess))
                {
                    Console.WriteLine("You've already tried '{0}', and it was correct!", guess);
                    continue;
                }
                else if (IncorrectLetters.Contains(guess))
                {
                    Console.WriteLine("You've already tried '{0}', and it was wrong!", guess);
                    continue;
                }

                //Replace each guessed letter on it's proper place of the displayed word.
                if (GuessedWordUpperCase.Contains(guess))
                {
                    GuessedLetters.Add(guess);

                    for (int i = 0; i < GuessedWord.Length; i++)
                    {
                        if (GuessedWordUpperCase[i] == guess)
                        {
                            DisplayWord[i] = GuessedWord[i];
                            revealedletters++;
                        }
                    }
                    Console.WriteLine(DisplayWord.ToString());

                }
                //If user has entered incorrect letter, system takes 1 life away and adds the letter to the list with incorrect guessed letters.
                else
                {
                    IncorrectLetters.Add(guess);

                    Console.WriteLine("Nope, there's no '{0}' in it!Attempts left: {1}", guess, lives - 1);
                    lives--;
                    Console.WriteLine(DisplayWord.ToString());
                }
                //When the number of revealed letters are equal to the total number of letters, player advances to level 2. 
                if (revealedletters == GuessedWord.Length)
                {
                    level = 2;
                    Console.BackgroundColor = ConsoleColor.Green;
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine("Congratulations! You have passed level 1!");
                    Console.ResetColor();
                }

                //Method, drawing the Hangman
                DrawHangman(lives);
            }

            if (level == 2)
            {
                //system clears the list with correct and incorrect guessed letters. Clears the displayed word, resets the number of revealed letters
                //and takes a new word from a different file, containing more complex words.
                GuessedLetters.Clear();
                IncorrectLetters.Clear();
                lives = 8;
                DisplayWord.Clear();
                revealedletters = 0;
                wordBank = File.ReadAllLines(@"..\..\Level2.txt");
                GuessedWord = wordBank[random.Next(0, wordBank.Length)];

                GuessedWordUpperCase = GuessedWord.ToUpper();
                DisplayWord = new StringBuilder(GuessedWord.Length);
                DrawWord(GuessedWord, DisplayWord);
                // START OF LEVEL 2
                while (lives > 0 && level == 2)
                {
                    //System repeats the same steps from level 1.
                    Console.Write("Guess a letter: ");

                    input = Console.ReadLine().ToUpper();
                    if (!string.IsNullOrWhiteSpace(input))
                    {
                        guess = input[0];
                    }
                    else
                    {
                        guess = ' ';
                    }

                    if (GuessedLetters.Contains(guess))
                    {
                        Console.WriteLine("You've already tried '{0}', and it was correct!", guess);
                        continue;
                    }
                    else if (IncorrectLetters.Contains(guess))
                    {
                        Console.WriteLine("You've already tried '{0}', and it was wrong!", guess);
                        continue;
                    }

                    if (GuessedWordUpperCase.Contains(guess))
                    {
                        GuessedLetters.Add(guess);

                        for (int i = 0; i < GuessedWord.Length; i++)
                        {
                            if (GuessedWordUpperCase[i] == guess)
                            {
                                DisplayWord[i] = GuessedWord[i];
                                revealedletters++;
                            }
                        }
                        Console.WriteLine(DisplayWord.ToString());
                    }

                    else
                    {
                        IncorrectLetters.Add(guess);

                        Console.WriteLine("Nope, there's no '{0}' in it!Attempts left: {1}", guess, lives - 1);
                        lives--;
                        Console.WriteLine(DisplayWord.ToString());
                    }

                    DrawHangman(lives);
                    //If the number of revealed letters is equal to the number of letters in the word for guessing, player advances to level 3.
                    if (revealedletters == GuessedWord.Length)
                    {
                        Console.BackgroundColor = ConsoleColor.Blue;
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Congratulations! You have passed level 2! Start of Level 3");
                        Console.ResetColor();
                        level++;
                    }


                }
            }
            if (level == 3)
            {

                //System repeats the same steps as from the start of level 3. It takes a new word from a file, filled with more complex words.
                GuessedLetters.Clear();
                IncorrectLetters.Clear();
                lives = 8;
                DisplayWord.Clear();
                revealedletters = 0;
                wordBank = File.ReadAllLines(@"..\..\Level3.txt");
                GuessedWord = wordBank[random.Next(0, wordBank.Length)];

                GuessedWordUpperCase = GuessedWord.ToUpper();
                DisplayWord = new StringBuilder(GuessedWord.Length);
                DrawWord(GuessedWord, DisplayWord);
                // START OF LEVEL 3
                //As this is the last level, system will perform the cycle until the player has lives and until he does not win.
                while (lives > 0 && !win && level == 3)
                {
                    Console.WriteLine();
                    Console.WriteLine("Guess a letter: ");
                    input = Console.ReadLine().ToUpper();

                    if (!string.IsNullOrWhiteSpace(input))
                    {
                        guess = input[0];
                    }
                    else
                    {
                        guess = ' ';
                    }

                    if (GuessedLetters.Contains(guess))
                    {
                        Console.WriteLine("You've already tried '{0}', and it was correct!", guess);
                        continue;
                    }
                    else if (IncorrectLetters.Contains(guess))
                    {
                        Console.WriteLine("You've already tried '{0}', and it was wrong!", guess);
                        continue;
                    }

                    if (GuessedWordUpperCase.Contains(guess))
                    {
                        GuessedLetters.Add(guess);

                        for (int i = 0; i < GuessedWord.Length; i++)
                        {
                            if (GuessedWordUpperCase[i] == guess)
                            {
                                DisplayWord[i] = GuessedWord[i];
                                revealedletters++;

                            }
                        }
                    }
                    else
                    {
                        IncorrectLetters.Add(guess);

                        Console.WriteLine("Nope, there's no '{0}' in it!Attempts left: {1}", guess, lives - 1);
                        lives--;

                    }
                    DrawHangman(lives);
                    //If the number of revealed letters are equal to the number of guessed letters, user wins.
                    if (revealedletters == GuessedWord.Length)
                    {
                        win = true;
                    }

                    Console.WriteLine(DisplayWord.ToString());
                }
            }
            //Text, appearing when the player wins. 
            if (win)
            {
                Console.BackgroundColor = ConsoleColor.Green;
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("Congratulations! You have won!");
                Console.ResetColor();

            }
            //Text, appearing when the player loses.
            else
            {
                Console.WriteLine("GAME OVER! The word was '{0}'", GuessedWord);
            }
        }
        //Method for drawing the word
        private static void DrawWord(string GuessedWord, StringBuilder DisplayWord)
        {
            for (int i = 0; i < GuessedWord.Length; i++)
            {
                DisplayWord.Append('_');
            }
            Console.WriteLine();
            Console.WriteLine(DisplayWord);
            Console.WriteLine();
        }

        //Method for drawing the Hangman. Each case in the switch represents the number of lives the player currently has.
        private static void DrawHangman(int lives)
        {
            switch (lives)
            {
                case 8:
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine();
                    Console.WriteLine();
                    break;
                case 7:
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" _________");
                    Console.WriteLine(" |       |");
                    Console.WriteLine(" |       |");
                    Console.WriteLine(" |_______|");
                    Console.WriteLine();
                    Console.WriteLine();
                    break;
                case 6:
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine(" ");
                    Console.WriteLine("     |    ");
                    Console.WriteLine("     |    ");
                    Console.WriteLine(" ____|____");
                    Console.WriteLine(" |       |");
                    Console.WriteLine(" |       |");
                    Console.WriteLine(" |_______|");
                    Console.WriteLine();
                    Console.WriteLine();
                    break;
                case 5:
                    Console.WriteLine("     _________");
                    Console.WriteLine("     |        ");
                    Console.WriteLine("     |        ");
                    Console.WriteLine("     |        ");
                    Console.WriteLine("     |        ");
                    Console.WriteLine("     |        ");
                    Console.WriteLine("     |        ");
                    Console.WriteLine("     |        ");
                    Console.WriteLine("     |        ");
                    Console.WriteLine(" ____|____    ");
                    Console.WriteLine(" |       |    ");
                    Console.WriteLine(" |       |    ");
                    Console.WriteLine(" |_______|    ");
                    Console.WriteLine();
                    Console.WriteLine();
                    break;
                case 4:
                    Console.WriteLine("     _________     ");
                    Console.WriteLine("     |       _|_   ");
                    Console.WriteLine("     |      /. .\\ ");
                    Console.WriteLine("     |      \\_-_/ ");
                    Console.WriteLine("     |        |    ");
                    Console.WriteLine("     |        |    ");
                    Console.WriteLine("     |        |    ");
                    Console.WriteLine("     |             ");
                    Console.WriteLine("     |             ");
                    Console.WriteLine(" ____|___          ");
                    Console.WriteLine(" |       |");
                    Console.WriteLine(" |       |");
                    Console.WriteLine(" |_______|");
                    Console.WriteLine();
                    Console.WriteLine();
                    break;
                case 3:
                    Console.WriteLine("     _________     ");
                    Console.WriteLine("     |       _|_   ");
                    Console.WriteLine("     |      /. .\\ ");
                    Console.WriteLine("     |      \\_-_/ ");
                    Console.WriteLine("     |        |    ");
                    Console.WriteLine("     |       /|    ");
                    Console.WriteLine("     |      / |    ");
                    Console.WriteLine("     |             ");
                    Console.WriteLine("     |             ");
                    Console.WriteLine(" ____|___          ");
                    Console.WriteLine(" |       |");
                    Console.WriteLine(" |       |");
                    Console.WriteLine(" |_______|");
                    Console.WriteLine();
                    Console.WriteLine();
                    break;
                case 2:
                    Console.WriteLine("     _________     ");
                    Console.WriteLine("     |       _|_   ");
                    Console.WriteLine("     |      /. .\\ ");
                    Console.WriteLine("     |      \\_-_/ ");
                    Console.WriteLine("     |        |    ");
                    Console.WriteLine("     |       /|\\  ");
                    Console.WriteLine("     |      / | \\ ");
                    Console.WriteLine("     |             ");
                    Console.WriteLine("     |             ");
                    Console.WriteLine(" ____|___          ");
                    Console.WriteLine(" |       |");
                    Console.WriteLine(" |       |");
                    Console.WriteLine(" |_______|");
                    Console.WriteLine();
                    Console.WriteLine();
                    break;
                case 1:
                    Console.WriteLine("     _________     ");
                    Console.WriteLine("     |       _|_   ");
                    Console.WriteLine("     |      /. .\\ ");
                    Console.WriteLine("     |      \\_-_/ ");
                    Console.WriteLine("     |        |    ");
                    Console.WriteLine("     |       /|\\  ");
                    Console.WriteLine("     |      / | \\ ");
                    Console.WriteLine("     |       /     ");
                    Console.WriteLine("     |      /      ");
                    Console.WriteLine(" ____|___          ");
                    Console.WriteLine(" |       |");
                    Console.WriteLine(" |       |");
                    Console.WriteLine(" |_______|");
                    Console.WriteLine();
                    Console.WriteLine();
                    break;
                case 0:
                    Console.WriteLine("     _________    ");
                    Console.WriteLine("     |       _|_  ");
                    Console.WriteLine("     |      /x x\\ ");
                    Console.WriteLine("     |      \\_-_/ ");
                    Console.WriteLine("     |        |   ");
                    Console.WriteLine("     |       /|\\  ");
                    Console.WriteLine("     |      / | \\ ");
                    Console.WriteLine("     |       / \\  ");
                    Console.WriteLine("     |      /   \\ ");
                    Console.WriteLine(" ____|___         ");
                    Console.WriteLine(" |       |");
                    Console.WriteLine(" |       |");
                    Console.WriteLine(" |_______|");
                    Console.WriteLine();
                    Console.WriteLine();
                    break;
            }
        }
    }
}
