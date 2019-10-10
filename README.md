# @luuc/react-d3-radar

<img src="http://i.imgur.com/rgJ7bXi.png" width="300">

React Radar Chart for D3 with Animations

## What is this?

Extension of Shauns [`react-d3-radar`](https://github.com/shauns/react-d3-radar) with animations.

## How does it work?
It animates the plot in 1 second from the `previousValues` to the `currentValues`. In the example below you can see how the `currentValues` can be passed on in order to have a continuous animation.

## Examples

```js
import Radar from '@luuc/react-d3-radar';

const Graph = ({ crowdMetas, currentTrackStats, artist, name }) => {
  const [previousCrowdValues, setPreviousCrowdValues] = useState({
    energy: 0.0, 
    acousticness: 0.0,
    danceability: 0.0,
  });

  const [currentCrowdValues, setCurrentCrowdValues] = useState({
    energy: 0.0, 
    acousticness: 0.0, 
    danceability: 0.0,
  });

  useEffect(() => {
    if (currentCrowdValues.energy !== crowdMetas.energy
      || currentCrowdValues.danceability !== crowdMetas.danceability
      || currentCrowdValues.acousticness !== crowdMetas.acousticness) {
      animateValueChanges();
    }
  })

  const animateValueChanges = () => {
    setPreviousCrowdValues({
      energy:  currentCrowdValues.energy,
      accusticness:  currentCrowdValues.acousticness,
      danceability: currentCrowdValues.danceability,
    });
    setCurrentCrowdValues({
      energy: crowdMetas.energy,
      acousticness: crowdMetas.acousticness,
      danceability: crowdMetas.danceability,
    });
  }

  return (
    <Container>
      <Radar
        width={500}
        height={500}
        padding={70}
        domainMax={1}
        highlighted={null}
        onHover={(point) => {
          if (point) {
            console.log('hovered over a data point');
          } else {
            console.log('not over anything');
          }
        }}
        data={{
          variables: [
            {key: 'energy', label: 'Energy'},
            {key: 'acousticness', label: 'Accousticness'},
            {key: 'danceability', label: 'Dancability'},
          ],
          sets: [
            {
              key: 'crowd',
              label: 'Crowd dancing',
              previousValues: previousCrowdValues,
              currentValues: currentCrowdValues,
            },
            {
              key: 'song',
              label:  `${name} - ${artist}`,
              previousValues: {
                energy: currentTrackStats.energy, 
                acousticness: currentTrackStats.acousticness,
                danceability: currentTrackStats.danceability,
              },
              currentValues: {
                energy: currentTrackStats.energy, 
                acousticness: currentTrackStats.acousticness,
                danceability: currentTrackStats.danceability,
              },
            },
          ],
        }}
      />
    </Container>
  )
}

export default Graph;
```
## API

### &lt;Radar />

Renders a Radar chart in SVG (creates its own `svg` element). Props are as per the example above.
