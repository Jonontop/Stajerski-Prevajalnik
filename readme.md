```py

import requests
from bs4 import BeautifulSoup

class Scraper:
    def __init__(self) -> None:
        self.__url: str = "https://slo-tech.com/forum/t100592"

    @property
    def scrape(self) -> dict:
        response = requests.get(self.__url)
        soup = BeautifulSoup(response.text, 'html.parser')
        content_div = soup.find('div', class_='content')
        br_tags = content_div.find_all('br')
        words_after_br: dict = {}

        for br in br_tags:
            next_node = br.find_next_sibling(text=True)

            if next_node:
                words_after_br.update({next_node.strip().split()[0].lower(): " ".join(next_node.strip().split()[1:]).lower()})

        return words_after_br

    @property
    def remove_letters(self) -> dict:

        words_after_br = self.scrape
        x = words_after_br.copy()

        for j in words_after_br.keys():
            if len(j) == 1:
                del x[j]

        return x


    def prevod(self, stavek: str) -> str:
        stavek = stavek.lower().split(" ")
        slovar = self.remove_letters

        for i in stavek:
            if i in slovar:
                stavek[stavek.index(i)] = slovar[i]

        return " ".join(stavek)


if __name__ == '__main__':
    scraper = Scraper()
    print(scraper.remove_letters)

```
