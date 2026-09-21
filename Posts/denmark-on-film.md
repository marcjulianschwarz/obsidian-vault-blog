---
blog-title: Denmark on Film
blog-subtitle: A Pentax ME, a roll of Kodak Gold, and a very slow week
blog-published: 2025-08-12
blog-tags:
  - EN
  - Photography
photos:
  - file: denmark-grassy-dunes.png
    title: Grassy Dunes
    description: Taken on a small danish island.
    location:
      country: Denmark
    camera:
      make: Pentax
      model: ME
      lens: 50mm f/1.8
      film: Kodak Gold 200
      settings:
        aperture: f/8
        shutter: 1/250
        iso: 200
  - file: denmark-boat.jpg
    title: Boat in Sonderborg
    description: The harbour was completely still that morning.
    location:
      city: Sonderborg
      country: Denmark
    camera:
      make: Pentax
      model: ME
      lens: 50mm f/1.8
      film: Kodak Gold 200
      settings:
        aperture: f/5.6
        shutter: 1/125
        iso: 200
  - file: denmark-sailboat.jpg
    title: Sailboat in Sonderborg
    location:
      city: Sonderborg
      country: Denmark
    camera:
      make: Pentax
      model: ME
      lens: 50mm f/1.8
      film: Kodak Gold 200
  - file: mossy-rooftop.png
    title: Mossy Rooftop
    description: This rooftop caught my attention by the amount of moss growing on it.
    gallery: false
    location:
      country: Denmark
    camera:
      make: Pentax
      model: ME
      lens: 50mm f/1.8
      film: Kodak Gold 200
---

I took one camera to Denmark this summer. No digital back-up body, no zoom, just a **Pentax ME** with a 50mm lens and four rolls of Kodak Gold 200. The constraint turned out to be the point.

## The first morning

The island was mostly dunes and wind. I walked the same stretch of beach three times before the light was worth a frame.

![Grassy dunes](/photos/denmark-grassy-dunes.png)

> Shooting film slows you down in a way that is hard to fake. You get 36 frames. You start asking whether a scene is actually interesting, or whether you just want to press the button.

## Sonderborg harbour

The harbour was flat calm. Two frames, one boat, and then the light shifted.

![Boat in the harbour](/photos/denmark-boat.jpg)

A few minutes later the sailboat drifted into the same spot.

![Sailboat](/photos/denmark-sailboat.jpg)

### What I carried

- Pentax ME body
- 50mm f/1.8
- Four rolls of Kodak Gold 200
- A light meter app, which I mostly ignored

The metering routine was simple enough to keep in my head:

```python
def sunny_16(iso: int, aperture: float) -> str:
    """Rough shutter speed for bright daylight."""
    stops = (aperture / 16) ** 2
    shutter = (1 / iso) * stops
    return f"1/{round(1 / shutter)}"


print(sunny_16(200, 8.0))
```

## One that did not make the gallery

This rooftop is in the post but marked `gallery: false`, so it stays here and does not show up in the overview.

![Mossy rooftop](/photos/mossy-rooftop.png)

That is the whole trip. Four rolls, about a dozen keepers, and a much better sense of when a photo is worth taking.
