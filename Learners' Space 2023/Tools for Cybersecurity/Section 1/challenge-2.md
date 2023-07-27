```python
import requests
  
def get_webpage(url):
    response = requests.get(url)
    return response.text
  
# print(get_webpage('https://www.cse.iitb.ac.in/~akshatka/'))
  
# extract new webpage from the old one
def extract_comment(text):
    import re
    comment = re.search("<!--(.*)-->", text, re.DOTALL)
    return comment.group(1)
  
def extract_link(text):
    import re
    link = re.search('https?:\/\/[^\s]+', text)
    return link.group(0)
  
url = 'https://www.cse.iitb.ac.in/~akshatka/'
webpage = get_webpage(url)
comment = extract_comment(webpage)
link = extract_link(comment)
# print(comment)
# print(link)
  
def linkfinder(initial_url):
    url = initial_url
    for i in range(100):
        try:
            webpage = get_webpage(url)
            comment = extract_comment(webpage)
            url = extract_link(comment)
            print(url)
        except:
            print('No link found')
            print('Last url: ', url)
            break
  
# linkfinder('https://www.cse.iitb.ac.in/~akshatka/')
  
import os
  
# Directory path
base_directory = 'C:/Users/Vivek/Downloads/chall2/inhere/'
  
# Function to find files with a specific size
def find_files_with_size(directory, target_size):
    matching_files = []
    for root, dirs, files in os.walk(directory):
        for file in files:
            file_path = os.path.join(root, file)
            if os.path.getsize(file_path) == target_size:
                matching_files.append(file_path)
    return matching_files
  
# Search for files with size 87369 bytes in each 'maybehere' folder
for folder_num in range(20):
    folder_name = f'maybehere{folder_num:02d}'
    folder_path = os.path.join(base_directory, folder_name)
  
    if os.path.isdir(folder_path):
        matching_files = find_files_with_size(folder_path, 87369)
        if matching_files:
            print(f"Files with size 87369 bytes in '{folder_name}' folder:")
            for file_path in matching_files:
                print(file_path)
            print()
```


