# Implement linear regression in React

[<img src="https://images.unsplash.com/photo-1456081101716-74e616ab23d8?auto=format&fit=crop&w=1510&q=80&ixid=dW5zcGxhc2guY29tOzs7Ozs%3D">](
https://unsplash.com/photos/wpi3sDUrSEk)
Photo by Dmitry Ratushny on Unsplash - https://unsplash.com/photos/wpi3sDUrSEk

Linear regression is one of the first concepts you have to get your head around when you start with machine learning. In a simple linear regression, scores are predicted on one variable from the scores on a second variable. Very simple and powerful. Here I implemented the concept with React and visualized it with React-vis. Enjoy

[Open Source Code on Github](https://github.com/DDCreationStudios/logisticRegression/tree/feature/forArticle) (Be aware that this is only a branch of the project I am working on)

The result can be seen here: https://ddcreationstudios.github.io/logisticRegression/ 


## 📄 Table of contents

  - [On linear regression](#on-linear-regression)
  - [Used data](#used-data)
  - [Implementation frameworks](#implementation-frameworks)
  - [Code walkthrough](#code-walkthrough)
    - [App.js](#appjs)
    - [Regression.js](#regressionjs)
    - [Plot.js](#plotjs)
  - [Further thoughts](#further-thoughts)


---
>“Control of consciousness determines the quality of life.” ― Mihaly Csikszentmihalyi
---


![gif](https://camo.githubusercontent.com/8702cf6f8016bc06f20490036fa028e065cf38cf/687474703a2f2f672e7265636f726469742e636f2f647271577035393139352e676966)

## On linear regression 

> In statistics, linear regression is a linear approach for modeling the relationship between a scalar dependent variable y and one or more explanatory variables (or independent variables) denoted X.

> Linear regression was the first type of regression analysis to be studied rigorously, and to be used extensively in practical applications. This is because models which depend linearly on their unknown parameters are easier to fit than models which are non-linearly related to their parameters and because the statistical properties of the resulting estimators are easier to determine.

> Linear regression has many practical uses. Most applications fall into one of the following two broad categories:If the goal is prediction, or forecasting, or error reduction, linear regression can be used to fit a predictive model to an observed data set of y and X values. After developing such a model, if an additional value of X is then given without its accompanying value of y, the fitted model can be used to make a prediction of the value of y.
Given a variable y and a number of variables X1, ..., Xp that may be related to y, linear regression analysis can be applied to quantify the strength of the relationship between y and the Xj, to assess which Xj may have no relationship with y at all, and to identify which subsets of the Xj contain redundant information about y.

[Source](https://en.wikipedia.org/wiki/Linear_regression#Introduction)

## Used data

Here I have used public available data from Austria. In this example the number of marriages of all people living in Salzburg (a city in Austria) are gathered over time. It allows to show differences in age, years, wifes or husbands.

## Implementation frameworks

For creating the web application I used React with the create-react-app boilerplate.
Visualization is done with Uber's React-vis library. It integrates nicely with a React app and allows a fast development of charts and plots. The regression calculation is done with Regression.js, a library for calculating the actual regression line and additional information.

## Code walkthrough

### App.js

The `index.js` provides the entry for the app. It renders `App.js`, which renders the following:

```jsx
//app.js
render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
        </header>
        <h5>Marriages in Salzburg over time</h5>
        <Plot
          regression={this.state.regression}
          data={prepareData(this.state.age)}
        />
        <div className="fixed-action-btn">
          <a className="btn-floating btn-large waves-effect waves-light red">
            {this.renderRegressionInfo()}
          </a>
        </div>

        <ControlPanel
          switchAge={arg => this.switchAge(arg)}
          calcRegression={this.renderRegression}
        />
        {this.state.regression ? (
          <div>
            Prediction for 2020:{" "}
            {calculateRegression(prepareData(this.state.age)).prediction[1]}
          </div>
        ) : (
          ""
        )}
        <Footer />
      </div>
    );
  }
```

In essence, it just calls the components, prepares the data, calculates the regression from the prepared data, and builds the plot accordingly.

### Regression.js

After the preparation of the data it can be easily used with the regression library to build the regression data:

```js
//regressions.js
import regression from "regression";

const calculateRegression = formattedData => {
  let regressionData = [];
  regressionData = formattedData.map(el => {
    return [parseFloat(el.x), el.y];
  });

  const result = regression.linear(regressionData);
  const gradient = result.equation[0];
  const yIntercept = result.equation[1];
  const prediction = result.predict(2020);

  regressionData = result.points.map(el => {
    return {
      x: el[0],
      y: el[1]
    };
  });
  return {regressionData, gradient, yIntercept, prediction};
};

export { calculateRegression as default };
```

### Plot.js

This is of course the heart of the app - the visualization. 

Since the regression curve always has to be calculated from the change in data I modularized it in an own function: 

```jsx
//Plot.jsx
const renderRegression = () => {
      if (this.props.regression) {
        return (
          <LineSeries
            data={calculateRegression(this.props.data).regressionData}
            color="red"
            animation={"gentle"}
            onNearestX={(value, { index }) =>
              this.setState({
                crosshairValues: [
                  calculateRegression(this.props.data).regressionData[index]
                ]
              })
            }
          />
        );
      }
    };
```

The rest of the Plot rendering is: 

```jsx
//Plot.jsx
return (
      <div className="container">
        <FlexibleWidthXYPlot
          height={400}
          onMouseLeave={() => this.setState({ crosshairValues: [] })}
        >
          <HorizontalGridLines />
          <VerticalGridLines />
          <MarkSeries
            data={this.props.data}
            onNearestX={this._rememberValue}
            animation={"gentle"}
          />
          {value ? (
            <LineSeries
              data={[{ x: value.x, y: value.y }, { x: XMAX, y: value.y }]}
              stroke="black"
            />
          ) : null}
          {value ? (
            <Hint value={value} getAlignStyle={getAlignStyle}>
              <div className="rv-hint__content">
                {`(Year ${value.x}, Marriages: ${value.y})`}
              </div>
            </Hint>
          ) : null}
          {renderRegression()}
          <XAxis top={0} hideTicks tickValues={years} title="X" />
          <XAxis title="Year" tickFormat={v => v} />
          <YAxis title="Number of Marriages" />
          <Crosshair
            values={this.state.crosshairValues}
            style={{
              line: { backgroundColor: "red" }
            }}
          >
            <div
              className="rv-hint__content"
              style={{ backgroundColor: "red" }}
            >
              <p>
                Year:{" "}
                {this.state.crosshairValues[0]
                  ? this.state.crosshairValues[0].x
                  : []}
              </p>
              <p>
                Marriages:{" "}
                {this.state.crosshairValues[0]
                  ? this.state.crosshairValues[0].y
                  : []}
              </p>
            </div>
          </Crosshair>
        </FlexibleWidthXYPlot>
      </div>
    );
```

Note: This return statement can/should be modularized as well, since it is not that clear from the start what it does. (Just realized this when writing this article :D)

As can be seen from the import statement 
```js 
import {
  FlexibleWidthXYPlot,
  MarkSeries,
  LineSeries,
  HorizontalGridLines,
  VerticalGridLines,
  XAxis,
  YAxis,
  Crosshair,
  Hint
} from "react-vis";
```
These are all react-vis components, which I have configured and adapted to my needs.

Key for rendering the Scatterplot or MarkSeries is this:
```jsx
<MarkSeries
            data={this.props.data}
            onNearestX={this._rememberValue}
            animation={"gentle"}
          />
```

The `data` is the prepared open source data passed down from the App.js component, `onNearestX` is used for the Crosshair, and the `animation` allows smooth movements

## Further thoughts

Now you have witnessed how easy it is to get going with visualizations in React. I encourage you to try it out and build your own application with it, since it can't really be any easier. 

I have also started to understand that many machine learning implementations are NOT restricted to Python. It's perfectly possible to start out with JavaScript. And this encourages me to do more with it. :)

Another important note: This is a very basic and not really sufficient example in terms of data outcome. This implementation of the linear regression is not suitable for timelines. Part of the linear regression is to find the point where the regression curve meets one axis. In this case this is simply not practical since it takes year 0 as the point of reference.

Stay motivated!

---

Thanks for reading my article! Feel free to leave any feedback! 

---

<!-- Written by Daniel Deutsch (deudan1010@gmail.com) -->
