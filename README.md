# Andmebaasi päringute ülesanded

# Ülesanne 21.1. Kõikide raamatute nimekiri.

SELECT * FROM books
;

# Ülesanne 21.2. Alates 2010. a välja antud raamatud, mis on kasutamata ja ei ole e-raamatud. Sorteerida pealkirja järgi tähestikulises järjekorras.

SELECT * FROM books
WHERE release_date > 2009
AND type = "new"
AND NOT type = "ebook"
ORDER BY title
;

# Ülesanne 21.3. Raamatud, mis on ilmunud enne 1970. aastat, on kasutatud ja mille hind on väiksem kui 20 eurot. Väljastada ainult pealkiri, aasta, hind ja tüüp veerud.

SELECT title, release_date, price, type FROM books
WHERE release_date < 1970
AND type = "used"
AND price < 20
;

# Ülesanne 21.4. Täidetud tellimuste arv aasta kaupa. Väljasta ainult tellimuse aasta ja tellimuste arv. Tulemuse veeru pealkirjaks pane “Aasta” ja “Tellimuste arv”.

SELECT COUNT(order_date) AS "Tellimuste arv", YEAR(order_date) AS "Aasta" FROM orders 
GROUP BY YEAR(order_date)
;

# Ülesanne 21.5. Täidetud tellimuste arv aasta kaupa ja müükide summa. Pane veergudele ilusad pealkirjad ja ümarda summa kahe komakohani.

SELECT YEAR(order_date) AS "Aasta", ROUND(SUM(price), 2) AS "Müük kokku", COUNT(order_date) AS "Tellimuste arv" 
FROM (SELECT order_date, b.price FROM orders AS o LEFT JOIN books AS b ON o.book_id =  b.id) o
GROUP BY YEAR(order_date)
;

# Ülesanne 21.6. Täidetud tellimuste arv viimase aasta jooksul ja müükide summa.

SELECT YEAR(order_date) as "Aasta", COUNT(order_date) as "Tellimuste arv", ROUND(SUM(price), 2) AS "Summa"
FROM orders
LEFT JOIN books 
ON orders.book_id = books.id
GROUP BY YEAR(order_date) DESC
LIMIT 1
;
