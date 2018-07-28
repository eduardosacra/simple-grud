# simple-grud

### A litle project about how implement a simple GRUD with Mysql using Syfmony4.1


## How to Run

Once you're done, simply `cd` to your project and run `docker-compose up -d`. This will initialise and start all the containers, then leave them running in the background.
Next step you go access the container php-fpm and install a depedencies with the commands:
* In my case the container is called `grud-simple_php-fpm_1`, then  excete `docker exec -it grud-simple_php-fpm_1 bash`
* `composer install`
* `bin/console doctrine:migrations:migrate`
 Done. Now you can usin um web browser of your preference, and access the endpoint `localchost:8000/'

### available routes
`/product/listing`
`/produtc/create`

#### Todo Routes
`/product/update/{id}`
`/product/delete/{id}`
`/product/info/{id}}`
