+++
title = 'Pokemon'
date = 2024-05-02T16:11:44-04:00
draft = false
+++


[Pokemon API](https://github.com/m-r-maxwell/pokemonAPI)

This was a fun one! This was created with a few different technologies.
1. Golang (Using the updated net/http package)
2. PostgreSQL (utilizing some of the json data storage)
3. Go Swagger
4. Python
5. Docker

Starting off, this was made primarily to explore the new net/http standard library package since it sort of eliminates the need for something I’m more familiar with such as [gorilla/mux](https://github.com/gorilla/mux). Next was Python since I could find JSON data of Pokemon-related things. But no one wants to write a bunch of repetitive SQL insert statements, so I leveraged python to do that for me and adjusted as needed.

Next came setting up the Golang API using microservices based on 3 different areas: Pokemon, items, and moves. This is not a complete list but a good starting point to show off some various routes and functions.

After that I set up a local db to interact with, so I could store the data that I used Python to generate for me. I decided that the next step was to set up a Dockerfile to build and run the database for anyone that wants to try running it.

Whenever I come back to this, I want to work on making a Docker Compose to run the API, the Swagger server, and the db all at the same time.

Lastly there is also swagger included but admittedly needs some work since it was a very quick test project.  

Some quick thoughts though, since it was about exploring an updated standard library package.
``` golang
func (ps *PokemonService) RegisterHandlers(mux *http.ServeMux) {
	mux.Handle("GET /pokemon", ps.GetPokemonList())
	mux.Handle("POST /pokemon", ps.GetPokemonByName())
	mux.Handle("POST /pokemon/type/{type}", ps.GetPokemonByType())
	mux.Handle("GET /pokemon/{id}", ps.GetPokemonByPokedexNumber())
	mux.Handle("GET /pokemon/abilities", ps.GetPokemonAbilities())
	mux.Handle("POST /pokemon/abilities", ps.GetPokemonAbilityByName())
	mux.Handle("POST /pokemon/new", ps.CreatePokemon())
}
```

This new native router is quite powerful, able to identify the action and is very easy to work with coming from gorilla/mux.