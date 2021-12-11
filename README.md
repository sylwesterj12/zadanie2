Aplikacja używa gotowe pliki Dockerfile

Aby uruchomić aplikację należy użyć polecenia 

docker compose up 





version: '3.7'  


services:
  nginx:
    build: nginx/  # budowanie Dockerfile nginx
    ports:          #ustawienie portu na którym ma nasłuchiwać aplikacja
      - "6666:80"
    networks:       # ustawienie połączeń kontenerów sieciowych
      - backend
      - frontend
    depends_on:   # definiowanie kolejności uruchamianych kontenerów
      - php
      - mysql
      - phpMyAdmin
    
  php:
    build: php/  # budowanie Dockerfile php
    networks:   #ustawienie połączenia kontenera w sieci backend
      - backend
  mysql:
    build:
      context: ./mysql  #ścieżka do katalogu zawierającego plik Dockerfile
      dockerfile: Dockerfile.debian     # budowanie Dockerfile mysql 
    restart: always         #zdefiniowanie polityki restartu bazy danych 
    environment:  #Zadeklarowanie zmiennej która określi hasło do konta administratora
      MYSQL_ROOT_PASSWORD: "root"
    volumes:      #przypisanie wolumenu
      - bazadanychV:/zad2
    networks: #ustawienie połączenia kontenera w sieci backend
      - backend   
  phpMyAdmin:
    image: phpmyadmin     #budowanie obrazu phpmyadmin
    restart: always
    ports:   #ustawienie portu na którym można połączyć się do bazy danych 
      - "6001:80"
    networks: #ustawienie połączenia kontenera w sieci backend
      - backend
      

networks: #deklaracja sieci
  backend:
  frontend:

volumes: #deklaracja wolumenów
  bazadanychV:
