# File Name Lister - C# Program

A simple C# console program that lists all files of a specified extension in a given folder and saves the list in a `file_list.txt` file.

## Features
- Prompts for a folder path and a file extension (e.g., `pdf`, `jpg`).
- Lists the files in the folder with the specified extension.
- Saves the list of filenames (without extensions) to a text file (`file_list.txt`) in the same folder.

## Usage

1. **Clone or download** the repository.
2. **Compile and run** the program:

    If you're using the .NET SDK, run:
    ```bash
    dotnet run
    ```

    Or if you're using just the `.cs` file, compile with:
    ```bash
    csc Program.cs
    ./Program.exe
    ```

3. **Follow the prompts**:
    - Provide the folder path.
    - Specify the file extension (e.g., `pdf`, `jpg`, or `*` for all files).

4. **Check the folder** for a new `file_list.txt` containing the filenames.

## Source Code

<details>
<summary>Click to view the full source</summary>

```csharp
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
