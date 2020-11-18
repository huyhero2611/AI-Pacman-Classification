# AI-Pacman-Classification

>1. Perceptron
  - Áp dụng công thức:
    y′=argmax(y′′)score(f,y′′)
    ```
    y = util.Counter()
    for label in self.weights.keys():
       y[label] = (trainingData[i]) * self.weights[label]
    arg_max = y.argMax()
    ```
  - if y' = y => nothing
    ```
    if arg_max == trainingLabels[i]:
        continue
    ```
  - else wy=wy+f  &  wy′=wy′−f
    ```
    self.weights[trainingLabels[i]] += trainingData[i]
    self.weights[arg_max] -= trainingData[i]
    ```
>2. Perceptron analysis
  - return 100 features with highest weight for that label
    ```
    self.weights[label].sortedKeys()[:100]
    ```
  - result is grap a
    ```
    return 'a'
    ```
>3. MIRA
  - xét actualLabel là trainingLabel hiện tại và predictedLabel bởi hàm phân tích classify
    ```
    for i, data in enumerate(trainingData):
                    actualLabel = trainingLabels[i]
                    predictedLabel = self.classify([data])[0]
    ```
  - if actual != predicted => áp dụng công thức τ=min(C,((wy′−wy)f+1)/(2 * f^2) và xét điều kiện update weight
    ```
    tau = min(c, ((self.weights[predictedLabel] - self.weights[actualLabel]) * f + 1.0) / (2.0 * (f * f)))
    f.divideAll(1.0 / tau)
    self.weights[actualLabel] += f 
    self.weights[predictedLabel] -= f 
    ```
  - ...
>4. Digit Feature Design
  - em không biết làm câu này ạ.
>5. Behavioral Cloning
  - tương tự perceptron với chữ số nhưng trả về action thay vì đưa ra bit nhị phân
  - áp dụng công thức: score(s,a)=w∗f(s,a)
    ```
    y = util.Counter()
    for label in trainingData[i][0].keys():
        y[label] = (trainingData[i][0][label]) * self.weights
    ```
  - với a′=argmax(a′′)score(s,a′′)
    ```
    arg_max = y.argMax()
    ```
  - xét điều kiện update weight w=w+f(s,a) & w=w−f(s,a′)
    ```
    if arg_max == trainingLabels[i]:
        continue
    else:
        self.weights += trainingData[i][0][trainingLabels[i]]
        self.weights -= trainingData[i][0][arg_max]
    ```
>6. Pacman Feature Design
  - thêm các successors cần thiết
    ```
    successor = state.generateSuccessor(0, action)
    ghostStates = successor.getGhostStates()
    pacmanPosition = successor.getPacmanPosition()
    foods = successor.getFood().asList()
    ```
  - thêm các features cho game:
    - stop agent:
      ```
      features['stop_now'] = (action == 'Stop')
      ```
    - food agent
      ```
      if len(foods) > 0:
        features['nearest_food'] = min(
            [(util.manhattanDistance(food, pacmanPosition)) for food in foods])
      ```
    - khoảng cách gần nhất so với quái và tổng số quái cho khoảng cách < 3
      ```
      features["nearest_ghost"] = min([(util.manhattanDistance(
        ghost.getPosition(), pacmanPosition)) for ghost in ghostStates])
      features["ghost_count"] = sum([(util.manhattanDistance(
        ghost.getPosition(), pacmanPosition) < 3) for ghost in ghostStates])
      ```
    - số quái đang k scare
      ```
      features['scare_times'] = sum(
        [(ghost.scaredTimer == 0) for ghost in ghostStates])
      ```
