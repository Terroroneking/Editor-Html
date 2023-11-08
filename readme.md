using System;
using System.Text;

namespace EditorHtml
{
    class Program
    {
        static string currentFileContent = string.Empty;

        static void Main(string[] args)
        {
            Console.Clear();
            Show();
        }

        static void Show()
        {
            Console.Clear();
            Console.BackgroundColor = ConsoleColor.White;
            Console.ForegroundColor = ConsoleColor.Black;
            DrawScreen();
            WriteOptions();
            var option = GetOption();

            HandleMenuOption(option);
        }

        static void DrawScreen()
        {
            Console.WriteLine("+------------------------------+");
            for (int lines = 0; lines <= 10; lines++)
            {
                Console.WriteLine("|                              |");
            }
            Console.WriteLine("+------------------------------+");
        }

        static void WriteOptions()
        {
            Console.SetCursorPosition(3, 0);
            Console.WriteLine("Editor Html");
            Console.SetCursorPosition(3, 1);
            Console.WriteLine("===============");
            Console.SetCursorPosition(3, 3);
            Console.WriteLine("Selecione uma opção abaixo");
            Console.SetCursorPosition(3, 5);
            Console.WriteLine("1 - Novo Arquivo");
            Console.SetCursorPosition(3, 6);
            Console.WriteLine("2 - Abrir");
            Console.SetCursorPosition(3, 7);
            Console.WriteLine("0 - Sair");
            Console.SetCursorPosition(3, 8);
            Console.WriteLine("Opção: ");
        }

        static short GetOption()
        {
            if (short.TryParse(Console.ReadLine(), out short option))
            {
                return option;
            }
            return -1; // Valor inválido
        }

        static void HandleMenuOption(short option)
        {
            switch (option)
            {
                case 1:
                    currentFileContent = ShowEditor();
                    break;
                case 2:
                    Console.WriteLine("View");
                    break;
                case 0:
                    if (!string.IsNullOrEmpty(currentFileContent))
                    {
                        Console.WriteLine("Deseja salvar o arquivo? (S para salvar, qualquer outra tecla para sair sem salvar)");
                        var key = Console.ReadKey().Key;
                        if (key == ConsoleKey.S)
                        {
                            SaveFile(currentFileContent);
                        }
                    }
                    Environment.Exit(0);
                    break;
                default:
                    Show();
                    break;
            }
        }

        static string ShowEditor()
        {
            Console.Clear();
            Console.BackgroundColor = ConsoleColor.White;
            Console.ForegroundColor = ConsoleColor.Black;
            Console.Clear();
            Console.WriteLine("Modo Editor");
            Console.WriteLine("===========");
            var file = new StringBuilder();

            ConsoleKey key;
            do
            {
                key = Console.ReadKey().Key;
                if (key == ConsoleKey.Backspace && file.Length > 0)
                {
                    file.Length--;
                    Console.Write(" \b");
                }
                else
                {
                    file.Append(key);
                }
            } while (key != ConsoleKey.Escape);

            Console.WriteLine("\n==================");
            Console.WriteLine("Deseja salvar o arquivo? (S para salvar, qualquer outra tecla para sair sem salvar)");

            key = Console.ReadKey().Key;
            if (key == ConsoleKey.S)
            {
                SaveFile(file.ToString());
            }

            return file.ToString();
        }

        static void SaveFile(string content)
        {
            Console.WriteLine("Digite o nome do arquivo para salvar:");
            var fileName = Console.ReadLine();
            if (!string.IsNullOrWhiteSpace(fileName))
            {
                System.IO.File.WriteAllText(fileName, content);
                Console.WriteLine("Arquivo salvo com sucesso!");
            }
            else
            {
                Console.WriteLine("Nome de arquivo inválido. O arquivo não foi salvo.");
            }
        }
    }
}
