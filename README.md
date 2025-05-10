using System;
using System.IO;

class Program
{
    static void Main()
    {
        Console.Title = "File Name Lister";
        Console.Write("Provide the folder path: ");
        string folderPath = Console.ReadLine();
        
        try
        {
            if (Directory.Exists(folderPath))
            {
                Console.Write("Specify the file extension to list (e.g., pdf; jpg; '*' lists all fiels): ");
                string extension = Console.ReadLine();
                extension = char.ToUpper(extension[0]) + extension.Substring(1);
                // Get all files in the folder
                string[] files = Directory.GetFiles(folderPath, "*." + extension);
                
                if (files.Length != 0)
                {
                    // Print the file list
                    Console.WriteLine("\n" + extension + " files in the folder:");
                    foreach (string file in files)
                    {
                        string fileName = Path.GetFileName(file);
                        Console.WriteLine(fileName);
                    }
                    // Remove path from filenames
                    string[] fileNames = Array.ConvertAll(files, Path.GetFileNameWithoutExtension);
                    // Define the output file path
                    string outputPath = Path.Combine(folderPath, "file_list.txt");

                    // Write file names to the output file
                    File.WriteAllLines(outputPath, fileNames);

                    Console.WriteLine($"\nFile list saved here: {folderPath}\\file_list.txt");
                }

                else
                {
                    Console.WriteLine("\nNo file with this extension in the folder.\n");
                }
            }

            else
            {
                Console.WriteLine("The specified folder does not exist.");
            }
            
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error: {ex.Message}");
        }
        Console.WriteLine("\nPress any button to quit...");
        Console.ReadKey();
    }
}
