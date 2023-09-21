# movies

from bs4 import BeautifulSoup
import requests

try:
    url = 'https://www.imdb.com/chart/top/'
    headers = {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36'}
    source = requests.get(url, headers=headers)
    source.raise_for_status()

    soup = BeautifulSoup(source.text, 'html.parser')

    movie_list = soup.find('ul', class_='ipc-metadata-list ipc-metadata-list--dividers-between sc-3f13560f-0 sTTRj compact-list-view ipc-metadata-list--base')
    movies = movie_list.find_all('li')

    for movie in movies:
        title = movie.find('li', class_='titleColumn')
        if title is not None:
            title = title.a.get_text()
        else:
            title = movie.find('h3').get_text()

        print(f'title:{title}  ')

except Exception as e:
    print(e)
