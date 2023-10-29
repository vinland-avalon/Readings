# What Happend When "Await" (asynchronized wait)
## When Compiling
1. The compiler rewrite the async method into a class, respresenting a state machine. Such rewriting will break code into "before await part" and "after await part". It also track the variables.
## When Reach the "await" Sentence
2. The method return, with a package of the following code -- "continuation".
## After the "await" Completed
3. The "continuation" will be scheduled to run.
## An Example
```CSharp
using System;
using System.Net.Http;
using System.Threading.Tasks;
using System.Windows.Forms;

public partial class YourForm : Form
{
    private async void downloadButton_Click(object sender, EventArgs e)
    {
        // Disable the button until the operation is complete
        downloadButton.Enabled = false;

        // Update the UI to indicate we're doing work
        statusLabel.Text = "Downloading...";

        // This is a non-blocking await. The UI thread is NOT blocked and can still respond to other events.
        string result = await DownloadDataAsync("https://www.example.com/data");

        // After the download is complete, the following lines of code will update the UI.
        // These lines will be executed on the UI thread due to the synchronization context captured by 'await'.
        statusLabel.Text = "Download complete!";
        resultTextBox.Text = result;

        // Enable the button after operation is complete
        downloadButton.Enabled = true;
    }

    // This simulates an asynchronous I/O-bound operation (like downloading from a server).
    private async Task<string> DownloadDataAsync(string url)
    {
        using (HttpClient client = new HttpClient())
        {
            // HttpClient's GetStringAsync method is naturally asynchronous and will not block the UI thread.
            string data = await client.GetStringAsync(url);
            return data;
        }
    }
}

```
## Note
- The keyword "async" is only to imply there may be asynchronization and await in this method. "await" is more important.
- The returned value of async method could be "void", "Task" and "Task<type>". The first and second both used to return no value indeed. The difference is, the "void" won't provide an event handler, which means it could not be "await"ed in a caller method. "fire and forget". We should avoid using "void".
- The "async/await" pattern is suitable for IO-bound tasks. It won't involve new background thread, but utilze ports provided by OS. If it's a CPU-bound task, we should use Task.run() in an async method. It is an encapsulation for thread-pool. For example
```CSharp
public async Task DoComplexCalculationAsync()
{
    // This line offloads the CPU-bound operation to a background thread.
    await Task.Run(() => 
    {
        // Some CPU-intensive calculation here.
    });

    // Other code here (after the await) will run on the original context, 
    // e.g., the UI thread for desktop applications.
}
```