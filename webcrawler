import queue
import threading
import requests
import re
import time
import csv


# Global variables
max_threads = 10  # maximum number of threads to use
visited_urls = set()  # set to keep track of visited URLs
urls_to_visit = queue.Queue()  # queue to keep track of URLs to visit
csv_file_name = "crawled_data.csv"


# Function to retrieve the HTML content of a page
def get_html(url):
    try:
        response = requests.get(url, timeout=5)
        return response.text
    except:
        return ""


# Function to extract all the links on a page
def get_links(html):
    links = re.findall(r'href=[\'"]?([^\'" >]+)', html)
    return links


# Function to visit a URL and extract all the links
def visit_url(url):
    global visited_urls, urls_to_visit
    html = get_html(url)
    links = get_links(html)
    visited_urls.add(url)
    for link in links:
        if link not in visited_urls:
            urls_to_visit.put(link)

    with open(csv_file_name, 'a', newline='') as f:
        writer = csv.writer(f)
        writer.writerow([url])


# Function to create and start threads to visit URLs
def start_threads():
    global urls_to_visit
    threads = []
    while not urls_to_visit.empty():
        # Create a new thread for each URL
        num_threads = min(max_threads, urls_to_visit.qsize())
        for i in range(num_threads):
            url = urls_to_visit.get()
            thread = threading.Thread(target=visit_url, args=(url,))
            threads.append(thread)

        # Start the threads
        for thread in threads:
            thread.start()

        # Wait for the threads to finish
        for thread in threads:
            thread.join()

        # Clear the threads list for the next iteration
        threads.clear()

        # Save the list of visited URLs to a file
        with open("visited_urls.txt", "w") as f:
            for url in visited_urls:
                f.write(url + "\n")


# Function to read the list of visited URLs from a file
def read_visited_urls():
    global visited_urls
    try:
        with open("visited_urls.txt", "r") as f:
            visited_urls = set(f.read().splitlines())
    except FileNotFoundError:
        pass


# Main function
def main():
    global urls_to_visit
    read_visited_urls()
    urls_to_visit.put("https://www.google.com")
    while not urls_to_visit.empty():
        start_time = time.time()
        start_threads()
        print(f"Visited {len(visited_urls)} URLs")
        print(f"Time elapsed: {time.time()-start_time:.2f} seconds")
        time.sleep(5)  # wait for 5 seconds before starting the next iteration


if __name__ == "__main__":
    main()
