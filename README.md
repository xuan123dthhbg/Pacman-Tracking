**Question 1: Exact Inference Observation**
## python pacman.py --frameTime 0 -p ReflexAgent -k 1

Xác suất ở mỗi vị trí mà ma có thể xuất hiện được tính bằng tích xác suất hậu nghiệm của tiếng ồn dựa trên khoảng cách manhattan của pacman với ma (P(noisyDistance|trueDistance)) nhân với xác suất tiên nghiệm mà ma xuất hiện ở mỗi vị trí (được cho bởi phân phối đều):

    if emissionModel[trueDistance] > 0:
      allPossible[p] = emissionModel[trueDistance] * self.beliefs[p]
                                                  
**Question 2: Exact Inference with Time Elapse**
## python autograder.py -q q2

Xác suất mà ma xuất hiện ở vị trí kế tiếp sẽ bằng xác suất ở vị trí hiện tại nhân với xác suất mà ma sẽ xuất hiện ở các vị trí tiếp theo(được cho bởi phân bố tùy vào loại ma). Hoặc có thể hiểu đây là xác suất có điều kiện P(vị trí tiếp theo | vị trí hiện tại của ma). Sau đó ta sẽ cập nhật xác suất này:

    for newPos, probability in positionDist.items():
      newBeliefs[newPos] += probability * self.beliefs[oldPos]
                                                                                                        
**Question 3: Exact Inference Full Test**
## python autograder.py -q q3

Tìm ra vị trí có khả năng xuất hiện nhất của mỗi ma và khoảng cách từ pacman đến những vị trí đó:

    mostLikeLyGhostPositions = []
            for index in range(len(livingGhostPositionDistributions)):
                highestProb, mostProbPosition = 0, None
                for position, prob in livingGhostPositionDistributions[index].items():
                    if prob > highestProb:
                        highestProb, mostProbPosition = prob, position
                mostLikeLyGhostPositions.append((mostProbPosition, self.distancer.getDistance(mostProbPosition, pacmanPosition)))
                                                      
 Sau đó tìm ra vị trí của ma gần nhất và khoảng cách từ pacman đến đó:
 
    leastDistance = float('inf')
      closestPosition = None
      for ghostPosition, distance in mostLikelyGhostPositions:
        if distance < leastDistance:
          leastDistance = distance
          closestPosition = ghostPosition
                                                          
 Cuối cùng chọn ra action tốt nhất:
 
    bestNewDistance = float('inf')
          bestAction = []
          for action in legal:
              successorPosition = Actions.getSuccessor(pacmanPosition, action)
              newDistance = self.distancer.getDistance(closestPosition, successorPosition)
              if newDistance < bestNewDistance:
                  bestNewDistance = newDistance
                  bestAction = [action]
              elif newDistance == bestNewDistance:
                  bestAction.append(action)
          return random.choice(bestAction)
          
**Question 4: Approximate Inference Observation**: khác Question 1 ở chỗ sử dụng tập mẫu để đưa ra suy diễn
## python autograder.py -q q4

Hàm *initializeUniformly():* 

	for counter in range(self.numParticles):
		self.particles.append(legalPositions[counter % len(legalPositions)])

Hàm *getBeliefDistribution():* Khởi tạo xác suất cho mỗi vị trí trong tập particles là bằng nhau và tổng bằng 1 (phân phối đều):

    for particle in self.particles:
        beliefDistribution[particle] += 1.0 / self.numParticles

Hàm *observe():* Tương tự hàm observe() của Question 1:

    allPossible = util.Counter()
                for p in self.legalPositions:
                    trueDistance = util.manhattanDistance(p, pacmanPosition)
                    if emissionModel[trueDistance] > 0:
                        allPossible[p] = emissionModel[trueDistance] * beliefDistribution[p]
                                                        
Khởi tạo lại tập mẫu nếu như xác suất ở tất cả các vị trí đều bằng 0:

    if allPossible.totalCount() == 0:
        self.initializeUniformly(gameState)

**Question 5: Approximate Inference with Time Elapse:** Tương tự Question 2
## python autograder.py -q q5

**Question 6: Joint Particle Filter Observation:**
## python autograder.py -q q6

**Question 7: Joint Particle Filter with Elapse Time:**
## python autograder.py -q q7
                                                  
                                                