# Word2Vec

Word2Vec: Algorithm for converting words to vector

Abstract
1. Skip-gram model is an efficient method for learning high-quality distributed vector representations that capture a large number of precise syntactic and semantic word relationships.
2. We present several extensions that improve both the quality of the vectors and the training speed.
3. By subsampling of the frequent words we obtain significant speedup and also learn more regular word representations.
4. We also describe a simple alternative to the hierarchical softmax called negative sampling.
5. We present a simple method for finding phrases in text, and show that learning good vector representations for millions of phrases is possible.

Methods
1. The Skip-gram Model
The training objective of the Skip-gram model is to find word representations that are useful for predicting the surrounding words in a sentence or a document.
More formally, given a sequence of training words w1, w2, … , wt, the objective of the Skip-gram model is to maximize the average log probability.

(1) where c is the size of the training context (which can be a function of the center word w_t). Larger c results in more training examples and thus can lead to a higher accuracy, at the expense of the training time. 
The basic Skip-gram formulation defines p(w_t+j|w_t) using the softmax function:

중심 단어(W_I)가 주어졌을 때, 주변단어(W_O)가 나타날 확률
V’_w의 transpose: W: input-hidden
V_wi: W’: hidden-output

Site: https://ratsgo.github.io/from%20frequency%20to%20semantics/2017/03/30/word2vec/

Sliding Window가 모든 단어를 학습하면 Iteration 1회가 마무리 됨.

1. The Skip-gram Model – 1.Hierarchical Softmax

2.Negative Sampling
Word2Vec은 Output Layer에서 출력하는 Score에 Softmax를 적용하여 확률 값으로 변환
-> 정답셋과 비교 -> Backpropagation 시행
But, Softmax를 적용할 시에 계산량이 많으므로 전체 단어를 대상으로 구하지 않고, 일부 단어만 뽑아서 계산함. -> Negative Sampling의 개념
Negative Sampling의 절차:
1. 사용자가 지정한 윈도우 사이즈 내에 등장하지 않는 단어(negative sample)를 5-20개 추출
2. 이를 정답단어와 합쳐 전체 단어처럼 Softmax 확률을 구함
즉, 윈도우 사이즈가 5일 경우 최대 25개 단어를 대상으로만 Softmax 확률을 계산하고, 파라미터 업데이트도 25개 대상으로만 이루어짐.

3.Subsampling of Frequent Words
단어 수가 늘어날수록 계산량이 증폭하는 구조임
Therefore, 말뭉치에서 자주 등장하는 단어는 학습량을 확률적인 방식으로 줄임.
(등장 빈도만큼 업데이트 될 기회가 많기 때문임)

I번째 단어(W_i)를 학습에서 제외시키기 위한 확률 식:

해당 단어가 말뭉치에 등장한 비율: F(w_i): 해당 단어 빈도/전체 단어 수
T는 사용자가 지정해 주는 값: 0.00001 권장

만약, f(w_i)가 0.01로 나타나는 빈도 높은 단어는 위 식으로 계산한 P(w_i)가 0.9684나 되어서 100번의 학습 기회 가운데 96번 정도는 학습에서 제외하게 됨.
-> Subsampling: 학습량을 효과적으로 줄여 계산량을 감소시키는 전략임.

Reference Journal: http://papers.nips.cc/paper/5021-distributed-representations-of-words-andphrases
Reference Code: https://gist.github.com/primaryobjects/8038d345aae48ae48988906b0525d175
