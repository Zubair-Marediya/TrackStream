# TrackStream

Check out the web app at: https://trackstream.herokuapp.com, but note that it's deprecated due to TuneFind API fees. Feel free to still check out the code above though!

You can hit the API we created at: http://trackstream-api.herokuapp.com/
- Here are the supported POST request routes: 
  - tunefind_get_movie_songs
  - tunefind_get_show_seasons
  - tunefind_get_show_episodes
  - tunefind_get_show_songs
  - youtube_search

Here's a bit about TrackStream:

TrackStream is a website where a user who wants to access songs from TV shows and movies they like can stream YouTube videos from specific episodes and films that are listed in the TuneFind API. A user would first choose either TV show or movie, then enter the title and hit the search bar. In the case of a movie, the full list of songs on the soundtrack (that are contained in the TuneFind API) would appear along with a description of the scene the song played during if available upon hovering over the button. The user can click on a specific song, and then the corresponding YouTube video for that song would appear on TrackStream for the user to view. In the case of a TV show, after hitting 'search' the user would be given a list of season numbers to choose from. After choosing a season number, the user is given a list of episodes by number and title, and once they choose an episode they're given the list of songs to choose from for streaming, again a description of the scene each song played during is available upon hovering over the button.

The underlying functionality of our app does the following: the client on a browser makes a get request to the root page of our web server where the basic home page "main.html" is returned. On that page if the user chooses to first interact by searching for a TV show, they can hit the "TV Show" button which will dynamically adjust the action of the form to make a request to the web server once the user hits search. Hitting this search sends the post request to the web server, which in turn, extracts the show name from the body using JSON (if the search is not blank) and sends that in a post request to the TrackStream API. From here, our API will take the show name by parsing the body, clean it using regular expressions, and query TuneFind's API for that show. Then, if TuneFind returns a valid response the API creates a JSON object with the show's season numbers as the keys and the values as lists containing the episode count and the season's TuneFind API URL.

Our API sends this JSON object back to our webserver where we have a helper function that constucts the HTML using the JSON object to populate a list of season buttons with values of the TuneFind API URL for each season. Using these URLs, the same process continues and repeats to retrieve and display the list of episodes for a given season, and the songs for a given episode. Finally, when the user selects a particular song from an episode, our web server concatenates the artist and song name and sends it to the TrackStream API which cleanses the query using regular expressions and sends a request to YouTube's API to query for the video ID of the top search result using these parameters. This video ID is sent back in a JSON object to the web server which constructs the HTML for an embedded YouTube link which is displayed as a video on the browser to the user.

Special thanks to Max Curran, Peitian Xiong, Shazia Ahmed, & Pooja Jain for helping create this!
