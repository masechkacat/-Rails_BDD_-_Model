
Quel est le nombre total d'objets Album contenus dans la base (sans regarder les id bien sûr) ?
`3.2.2 :001 > Album.all.last.id
  Album Load (8.6ms)  SELECT "albums".* FROM "albums" ORDER BY "albums"."id" DESC LIMIT ?  [["LIMIT", 1]]
 => 347 `

 Qui est l'auteur de la chanson "White Room" ?
 `3.2.2 :003 > Track.find_by(title: "White Room").artist
  Track Load (3.9ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."title" = ? LIMIT ?  [["title", "White Room"], ["LIMIT", 1]]
 => "Eric Clapton"`

 Quelle chanson dure exactement 188133 milliseconds ?

 `3.2.2 :004 > Track.find_by(duration: 188133).title
  Track Load (1.1ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."duration" = ? LIMIT ?  [["duration", 188133], ["LIMIT", 1]]
 => "Perfect" `

 Quel groupe a sorti l'album "Use Your Illusion II" ?

 3.2.2 :005 > Album.find_by(title: "Use Your Illusion II").artist
  Album Load (1.4ms)  SELECT "albums".* FROM "albums" WHERE "albums"."title" = ? LIMIT ?  [["title", "Use Your Illusion II"], ["LIMIT", 1]]
 => "Guns N Roses"

 Combien y a t'il d'albums dont le titre contient "Great" ?

 `3.2.2 :006 > Album.where("title LIKE ?", "%#{"Great"}%").length
  Album Load (1.6ms)  SELECT "albums".* FROM "albums" WHERE (title LIKE '%Great%')
 => 13 `

 Supprime tous les albums dont le nom contient "music".


 3.2.2 :007 > Album.where("title LIKE ?", "%#{"music"}%").destroy_all
  `Album Load (1.3ms)  SELECT "albums".* FROM "albums" WHERE (title LIKE '%music%')
  TRANSACTION (0.2ms)  begin transaction
  Album Destroy (6.0ms)  DELETE FROM "albums" WHERE "albums"."id" = ?  [["id", 315]]
  TRANSACTION (2.1ms)  commit transaction
  TRANSACTION (0.1ms)  begin transaction
  Album Destroy (4.3ms)  DELETE FROM "albums" WHERE "albums"."id" = ?  [["id", 319]]
  TRANSACTION (1.8ms)  commit transaction
  TRANSACTION (0.1ms)  begin transaction
  Album Destroy (5.0ms)  DELETE FROM "albums" WHERE "albums"."id" = ?  [["id", 333]]
  TRANSACTION (1.8ms)  commit transaction
  TRANSACTION (0.1ms)  begin transaction
  Album Destroy (4.1ms)  DELETE FROM "albums" WHERE "albums"."id" = ?  [["id", 346]]
  TRANSACTION (1.7ms)  commit transaction
 => 
[#<Album:0x00007ff45cab8850
  id: 315,
  title: "Handel: Music for the Royal Fireworks (Original Version 1749)",
  artist: "English Concert & Trevor Pinnock",
  created_at: Wed, 25 Oct 2023 15:05:25.909430000 UTC +00:00,
  updated_at: Wed, 25 Oct 2023 15:05:25.909430000 UTC +00:00>,
 #<Album:0x00007ff45cab8710
  id: 319,
  title: "Armada: Music from the Courts of England and Spain",
  artist: "Fretwork",
  created_at: Wed, 25 Oct 2023 15:05:25.966178000 UTC +00:00,
  updated_at: Wed, 25 Oct 2023 15:05:25.966178000 UTC +00:00>,
 #<Album:0x00007ff45cab85d0
  id: 333,
  title: "Purcell: Music for the Queen Mary",
  artist:
   "Equale Brass Ensemble, John Eliot Gardiner & Munich Monteverdi Orchestra and Choir",
  created_at: Wed, 25 Oct 2023 15:05:26.158910000 UTC +00:00,
  updated_at: Wed, 25 Oct 2023 15:05:26.158910000 UTC +00:00>,
 #<Album:0x00007ff45cab8490
  id: 346,
  title: "Mozart: Chamber Music",
  artist: "Nash Ensemble",
  created_at: Wed, 25 Oct 2023 15:05:26.335070000 UTC +00:00,
  updated_at: Wed, 25 Oct 2023 15:05:26.335070000 UTC +00:00>] `

  Combien y a t'il d'albums écrits par AC/DC ?
  `3.2.2 :010 > tp Album.where(artist: "AC/DC")
  Album Load (1.4ms)  SELECT "albums".* FROM "albums" WHERE "albums"."artist" = ?  [["artist", "AC/DC"]]
ID | TITLE                          | ARTIST | CREATED_AT              | UPDATED_AT             
---|--------------------------------|--------|-------------------------|------------------------
1  | For Those About To Rock We ... | AC/DC  | 2023-10-25 15:05:21     | 2023-10-25 15:05:21    
4  | Let There Be Rock              | AC/DC  | 2023-10-25 15:05:21     | 2023-10-25 15:05:21    
  3.2.2 :008 > Album.where(artist: "AC/DC").length
  Album Load (2.9ms)  SELECT "albums".* FROM "albums" WHERE "albums"."artist" = ?  [["artist", "AC/DC"]]
 => 2 `

 Combien de chanson durent exactement 158589 millisecondes ?

`3.2.2 :009 > Track.where(duration: 158589).length
  Track Load (2.4ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."duration" = ?  [["duration", 158589]]
 => 0 `

 puts en console tous les titres de AC/DC.
 `3.2.2 :011 > Album.where(artist: "AC/DC").each do |album|
3.2.2 :012 >   puts album.title
3.2.2 :013 > end
  Album Load (1.0ms)  SELECT "albums".* FROM "albums" WHERE "albums"."artist" = ?  [["artist", "AC/DC"]]
For Those About To Rock We Salute You
Let There Be Rock`

puts en console tous les titres de l'album "Let There Be Rock".
`3.2.2 :014 > Track.where(album:  "Let There Be Rock").each do |track|
3.2.2 :015 >   puts track.title
3.2.2 :016 > end
  Track Load (1.9ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."album" = ?  [["album", "Let There Be Rock"]]
Go Down
Dog Eat Dog
Let There Be Rock
Bad Boy Boogie
Problem Child
Overdose
Hell Aint A Bad Place To Be
Whole Lotta Rosie`

Calcule le prix total de cet album ainsi que sa durée totale
`3.2.2 :021 > puts Track.where(album:  "Let There Be Rock").reduce(0) { |sum, track| sum + track.price }.round(2)
  Track Load (1.1ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."album" = ?  [["album", "Let There Be Rock"]]
7.92
3.2.2 :023 > puts Track.where(album:  "Let There Be Rock").reduce(0) { |sum, track| sum + track.duration }/60000
  Track Load (1.6ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."album" = ?  [["album", "Let There Be Rock"]]
40
`

Calcule le coût de l'intégralité de la discographie de "Deep Purple".

`3.2.2 :024 > puts Track.where(artist: "Deep Purple").reduce(0) { |sum, track| sum + track.price }.round(2)
  Track Load (1.5ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."artist" = ?  [["artist", "Deep Purple"]]
90.09`

Modifie (via une boucle) tous les titres de "Eric Clapton" afin qu'ils soient affichés avec "Britney Spears" en artist.

`Track.where(artist: "Eric Clapton").each {|track| track.update(artist: "Britney Spears")}`