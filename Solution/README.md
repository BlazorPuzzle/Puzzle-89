# Puzzle #89 - Lighten the Load

Carl and Jeff want to know how to make a type-ahead search mechanism more efficient.

YouTube Video: https://youtu.be/

Blazor Puzzle Home Page: https://blazorpuzzle.com

## The Challenge

This is a .NET 9 Blazor Web App with Global Server Interactivity.

We have an input box bound to a search string. On every keystroke it filters a list by the search term, which can be very chatty, especially after the first character which may return hundreds or even thousands of records.

How can we make this mechanism more efficient to lighten the load on the back end?

## The Solution

The solution is to "de-bounce" the input. This is a technique that absorbs the first few (usually 2) keystrokes and then waits for a natural pause in typing before applying the filter.

We've added these locals:

```c#
private System.Threading.Timer? debounceTimer;
private readonly int debounceDelayMs = 300;
private readonly int minLength = 2;
```

And then instead of calling `ApplyFilter()` on every keystroke we perform this action:

```c#
private void OnInputChanged(ChangeEventArgs e)
{
    searchText = e.Value?.ToString() ?? string.Empty;
    Logger.LogInformation("Search text changed to: {SearchText}", searchText);
    if (string.IsNullOrWhiteSpace(searchText) || searchText.Length < minLength)
    {
        filteredEpisodes = new();
        selectedEpisode = null;
        InvokeAsync(StateHasChanged);
        return;
    }
    debounceTimer?.Dispose();
    debounceTimer = new System.Threading.Timer(_ =>
    {
        InvokeAsync(() => ApplyFilter());
    }, null, debounceDelayMs, System.Threading.Timeout.Infinite);
}
```

First, we bail if the string is empty or less than 2 characters (`minLength`).

Next. we use the `deBounceTimer` to wait 300ms (`debounceDelayMs`) before calling `ApplyFilter()`.

The result is a more efficient filtering mechanism that takes the load off of the back-end.

Boom!
