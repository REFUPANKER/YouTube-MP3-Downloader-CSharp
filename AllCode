using System;
using System.IO;
using System.Net;
using System.Threading.Tasks;
using VideoLibrary;
using MediaToolkit.Model;
using MediaToolkit;
using System.Configuration;
using System.Diagnostics;
namespace Mp3DownoaderTest
{
    class Program
    {
        static void Main(string[] args)
        {
            cwl("----YouTube Video Downloader----");
            DownloadMP3(crl("Specify Video URL : "));
        }
        static object cwl(object msg = null)
        {
            Console.WriteLine(msg);
            return msg;
        }
        static object cw(object msg = null)
        {
            Console.Write(msg);
            return msg;
        }
        static string crl(object msg = null)
        {
            Console.Write(msg);
            return Console.ReadLine();
        }
        public static string BasePath { get {
                return Environment.CurrentDirectory + "\\"; }
            set {
            } }
        public static string DownloadLocation {get{
                if (Directory.Exists(BasePath+ "Output\\")==false)
                {
                    Directory.CreateDirectory(BasePath + "Output\\");
                    Console.WriteLine("Dir Created");
                }
                return BasePath + "Output\\";
            } set{}}
        static void DownloadMP3(string videoURL)
        {
            try
            {
                var Yt = YouTube.Default.GetVideo(videoURL);
                var cutVideoName = Yt.FullName.Replace(" ", "_");
                cutVideoName = cutVideoName.Substring(0, cutVideoName.LastIndexOf("."));
                var videoPath = DownloadLocation + Yt.FullName.Replace(" ", "_");
                if (File.Exists(videoPath) || File.Exists(DownloadLocation + $"{cutVideoName}.mp3"))
                {
                    cwl("File already exists");
                    cwl(DownloadLocation + $"{cutVideoName}.mp3");
                    return;
                }
                cwl(Yt.FullName.Substring(0, Yt.FullName.LastIndexOf(".")));
                cwl(cutVideoName);
                cwl(videoPath);
                File.WriteAllBytes(videoPath, Yt.GetBytes());
                var inputFile = new MediaFile { Filename = videoPath };
                var outputFile = new MediaFile { Filename = DownloadLocation + $"{cutVideoName}.mp3" };
                using (var engine = new Engine())
                {
                    engine.GetMetadata(inputFile);
                    engine.ConvertProgressEvent += Engine_ConvertProgressEvent;
                    engine.ConversionCompleteEvent += Engine_ConversionCompleteEvent;
                    engine.Convert(inputFile, outputFile);
                    
                }
                File.Delete(videoPath);
            }
            catch (Exception ex)
            {
                cwl(ex.StackTrace.ToLower().Substring(ex.StackTrace.LastIndexOf("line")).ToUpper()+ex.Message);
            }
        }

        private static void Engine_ConversionCompleteEvent(object sender, ConversionCompleteEventArgs e)
        {
            Console.Title = "Convert Completed !";
        }

        private static void Engine_ConvertProgressEvent(object sender, ConvertProgressEventArgs e)
        {
            Console.Title = "Downloaded :"+e.SizeKb / 1024+" MB ";
        }
    }
}
