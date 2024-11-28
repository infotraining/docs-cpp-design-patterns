# Factories

> Prefer loosely coupled classes
> -- Principle of good OOP design

One of the principle of good Object-Oriented Programming says to prefer loose coupling between objects (classes).

The code presented below breaks this principle:

```c++
class MusicApp
{
public:
    //...

    void play(const std::string& track_title)
    {
        // creation of the object
        SpotifyService music_service("spotify_user", "rjdaslf276%2", 45);
        
        // usage of the object
        std::optional<Track> track = music_service.get_track(track_title);
        if (track.has_value())
        {
            music_service.play(track->id());
        }
        else
        {
            //... handle error
        }
    }
    
    //...rest of the class
};
```

`MusicApp` is responsible for creating an instance of `SpotifyService` and using it. This creates a tight coupling between `MusicApp` and `SpotifyService`. To create a new instance of `SpotifyService`, `MusicApp` needs to know the constructor arguments of `SpotifyService`. This is additional responsibility for `MusicApp`. If the constructor of `SpotifyService` changes, `MusicApp` needs to be updated as well.

After constructing the object, `MusicApp` uses the object to get a track. This usage should be separated from the creation of the object. This is where factories come into play.

A factory is an object that creates other objects. Its main responsibility is to encapsulate the object creation process. This allows the client code to create objects without knowing the concrete class of the object being created and without needing to know the constructor arguments required for the object's creation.

## Scenarios for Using Factories

Factories can be used in three distinct scenarios:

1. When we want to free the client code from knowing the concrete class that will be created.
2. When we want to create an object based on some parameter (e.g., an ID of type `std::string` or `enum`). This parameter can be known at runtime.
3. When the type of object to be created depends on the type of object that is already created.