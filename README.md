A simple multithreaded web crawler that visits URLs, extracts links from the pages, and saves the visited URLs to a CSV file. This crawler is implemented in Python and uses threading to improve crawling speed.

**Features**

Multithreading: Uses multiple threads to visit URLs concurrently.
Queue Management: Maintains a queue of URLs to visit and a set of visited URLs to avoid revisiting.
Data Persistence: Saves visited URLs to a file to allow resumption of the crawling process.
Link Extraction: Extracts all hyperlinks from visited pages.
CSV Logging: Logs all visited URLs to a CSV file.

**Requirements**

Python 3.x
requests library

Install the required libraries:

pip install requests

The initial URL to start crawling from is set to "https://www.google.com" by default. You can change this in the main() function.

**Global Variables**

max_threads: Maximum number of threads to use.

visited_urls: Set to keep track of visited URLs.

urls_to_visit: Queue to keep track of URLs to visit.

csv_file_name: Name of the CSV file where the crawled data will be saved.

**Functions**
get_html(url): Retrieves the HTML content of a page.

get_links(html): Extracts all links from the HTML content of a page.

visit_url(url): Visits a URL, extracts links, updates visited URLs, and logs to CSV.

start_threads(): Manages thread creation, execution, and synchronization.

read_visited_urls(): Reads the list of visited URLs from a file.

main(): Main function that initializes and manages the crawling process.
