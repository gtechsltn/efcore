# EF Core

Setup Entity Framework Core in Console Application, Code First

https://medium.com/@robrich22/setup-entity-framework-core-in-console-application-code-first-ad130b53a539

### NuGet
```
Microsoft.EntityFrameworkCore
Microsoft.EntityFrameworkCore.SqlServer
```

### Entity
```
public class Movie
{
    public int Id { get; set; }       
    public string Title { get; set; }
    public string Description { get; set; }
}
```

### Create Database DbContext
```
public class MovieContext : DbContext
{
    public DbSet<Movie> Movies { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer("Data Source=(localdb)\\MSSQLLocalDB;Initial Catalog=Movies;Integrated Security=True;Encrypt=False;");
    }
}
```

### Create your database
```
using (MovieContext context = new())
{
    context.Database.EnsureCreated();
}
```

### Insert a movie into database table
```
using (MovieContext context = new())
{
    var movie = new Movie()
    {
        Title = "Rambo First Blood",
        Description = "An ex-Green Beret haunted by memories of Vietnam, he was once the perfect killing machine. " +
                      "Now he's searching for peace, but finds instead an over-zealous, small-town sheriff who's " +
                      "spoiling for a fight."
    };

    context.Movies.Add(movie);
    context.SaveChanges();
}
```

### Insert many movies into the database table
```
using (MovieContext context = new())
{
    for (int x = 0; x < 100; x++)
    {
        var movie = new Movie()
        {
            Title = $"Rambo {x}",
            Description = $"Explosiove Action Movie part {x}"
        };

        context.Movies.Add(movie);
    }
    context.SaveChanges();
}
```

### Retrieve a movie from the database table
```
using (MovieContext context = new())
{
    var movie = context.Movies.FirstOrDefault();
    Console.WriteLine("Description: {0}", movie.Description);
}
```

### Retrieve many movies from the database table
```
using (MovieContext context = new())
{
    var movies = context.Movies.ToList();

    foreach (var movie in movies)
    {
        Console.WriteLine("{0}, {1}", movie.Title, movie.Description);
    }
}
```

### Update a movie in the database
```
using(MovieContext context = new())
{
    var movie = context.Movies.FirstOrDefault();
    movie.Title = movie.Title + " UPDATED!";
    context.SaveChanges();
}
```
### Delete a movie from the database
```
using(MovieContext context = new())
{
    var movie = context.Movies.FirstOrDefault();
    context.Movies.Remove(movie);
    context.SaveChanges();
}
```
