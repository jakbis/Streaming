using System;
using System.Collections.Generic;
using System.IO;
using System.Text;
using System.Threading;

class Test
{

    public static void Main()
    {
        string path = @"c:\temp\MyTest.txt";

        try
        {
            if (File.Exists(path))
            {
                File.Delete(path);
            }

            using (StreamWriter sw = new StreamWriter(path))
            {
                sw.WriteLine("This");
                sw.WriteLine("is some text");
                sw.WriteLine("to test");
                sw.WriteLine("Reading\n");
                sw.WriteLine("<");
                sw.WriteLine("blaaaaaaa");
                sw.WriteLine("bla@bla>     ");
                sw.WriteLine("is some text");
                sw.WriteLine("to test");
            }

            StringBuilder stringBuilder = new StringBuilder();
            string fixedstr = string.Empty;
            using (StreamReader sr = new StreamReader(path))
            {
                string prompt = "bla@bla>";
                char[] c = new char[prompt.Length];
                string line = string.Empty;
                DateTime now = DateTime.Now;
                DateTime timetosearch = DateTime.Now.AddMilliseconds(4000);
                bool flag = false;
                while (!line.Contains(prompt) && now < timetosearch)
                {
                    sr.Read(c, 0, c.Length);
                    string s = new string(c);
                    line += s;
                    stringBuilder.Append(c);
                    now = DateTime.Now;
                    if (line.Contains(prompt))
                        flag = true;
                }
                if (flag)
                {
                    int index = stringBuilder.ToString().IndexOf(prompt);
                    fixedstr = stringBuilder.ToString().Substring(0, index + prompt.Length);
                    Console.WriteLine(fixedstr);
                }
                else Console.WriteLine("NOT FOUND");
            }
        }
        catch (Exception e)
        {
            Console.WriteLine("The process failed: {0}", e.ToString());
        }
    }
}
