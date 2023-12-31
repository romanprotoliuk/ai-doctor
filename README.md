# Brainstorm session for LLM model interaction with raw health data

## Cosine Similarity - A Deep Dive

Cosine similarity is a metric used to determine how similar the documents are irrespective of their size. The cosine of 0° is 1, and it is less than 1 for any angle in the interval (0,π] radians. It is thus a judgment of orientation and not magnitude: two vectors with the same orientation have a cosine similarity of 1, two vectors oriented at 90° relative to each other have a similarity of 0, and two vectors diametrically opposed have a similarity of -1, independent of their magnitude.

## JavaScript Implementation

Here is a simple implementation of the cosine similarity algorithm in JavaScript:

```javascript
function dotProduct(vecA, vecB) {
    let product = 0;
    for (let i = 0; i < vecA.length; i++) {
        product += vecA[i] * vecB[i];
    }
    return product;
}

function magnitude(vec) {
    let sum = 0;
    for (let i = 0; i < vec.length; i++) {
        sum += vec[i] * vec[i];
    }
    return Math.sqrt(sum);
}

function cosineSimilarity(vecA, vecB) {
    return dotProduct(vecA, vecB) / (magnitude(vecA) * magnitude(vecB));
}
```

## Use Cases
Cosine similarity has a great range of applications thanks to its focus on orientation rather than magnitude. It can be used in:
**Information Retrieval**: Search engines can use cosine similarity to rank the relevance of a document to a given search query.
**Recommendation Systems**: Online platforms like Netflix or Amazon use cosine similarity to recommend products or movies similar to the user's past preferences.
**Natural Language Processing**: In semantic analyses, cosine similarity can help determine the similarity of meanings of two documents.
**Data Science and Machine Learning**: Cosine similarity is often used in clustering, classification, or anomaly detection tasks.

## Practical Example
For the vectors vecA = [1, 2, 3] and vecB = [4, 5, 6], their cosine similarity can be calculated as follows:

```javascript
let vecA = [1, 2, 3];
let vecB = [4, 5, 6];
let similarity = cosineSimilarity(vecA, vecB);

console.log("The cosine similarity between vecA and vecB is: " + similarity);
```
**When you run this script, you will get an output similar to:**

The cosine similarity between vecA and vecB is: 0.9746316794169463
This result indicates the two vectors are very similar as the cosine similarity is very close to 1.

## Real-World Application Example
Consider a content recommendation engine, such as those used by YouTube, Netflix, or other content streaming platforms. Each video on the platform could be represented by a multi-dimensional vector that encapsulates various features of the video (such as genre, length, director, etc.).

In this case, vecA and vecB could represent two different videos. A high cosine similarity (like 0.9746316794169463) would indicate that these two videos are very similar based on the features we have chosen. So, if a user watches and enjoys the video represented by vecA, our recommendation engine could then suggest the video represented by vecB to the user, with a reasonable expectation that they will also enjoy that video because it's similar to the one they've already watched and liked.

## Conclusion
Cosine similarity is a powerful metric used in various fields, from data science to natural language processing, and it plays a crucial role in providing personalized experiences in modern-day applications. By understanding the algorithm and its practical applications, we can better leverage this tool to deliver more meaningful and tailored results.


# ChatGPT Style custom data search architecture 
![1_aP8m4U_BzAe-dYDyzjS7EQ](https://github.com/romanprotoliuk/cosine-similarity/assets/45060965/a12a5834-9df0-4655-ab34-6a1934169aa0)

## Useful Tools/Tech stack

- [**pgvector**](https://github.com/romanprotoliuk/pgvector): A PostgreSQL extension for efficient vector computations.
- [**langchain**](https://js.langchain.com/docs/): A powerful tool for natural language processing in JavaScript.
- [**openai**](https://openai.com/)
- [**NextJS**](https://nextjs.org/)
- [**supabase**](https://supabase.com/)


## Caching for embedding 

```javascript
class EmbeddingsCache {
  constructor() {
    this.cache = new Map();
    this.pendingRequests = new Map();
  }

  async getEmbeddings(key, fetcher) {
    if (this.cache.has(key)) {
      return this.cache.get(key);
    }

    if (!this.pendingRequests.has(key)) {
      this.pendingRequests.set(key, fetcher(key).then(data => {
        this.pendingRequests.delete(key);
        this.cache.set(key, data);
        return data;
      }));
    }

    return this.pendingRequests.get(key);
  }
}

async function main() {
  const cache = new EmbeddingsCache();
  const apiKey = "YOUR_API_KEY";

  try {
    const key = "example_key";

    const embeddings = await cache.getEmbeddings(key, (key) => fetchEmbeddingsFromAPI(key, apiKey));

    processEmbeddings(embeddings);

  } catch (e) {
    console.log("An error occurred:", e.message);
  }
}

async function fetchEmbeddingsFromAPI(key, apiKey) {
  const url = "https://api.openai.com/v1/engines/text-davinci-003/completions";
  const prompt = `Embed the text: "${key}"`;

  const response = await makeAPIRequest(url, prompt, apiKey);

  const embeddings = parseEmbeddings(response);

  return embeddings;
}

async function makeAPIRequest(url, prompt, apiKey) {
  throw new Error("makeAPIRequest is not implemented");
}

function parseEmbeddings(response) {
  throw new Error("parseEmbeddings is not implemented");
}

function processEmbeddings(embeddings) {
  console.log("Processing embeddings:", embeddings);
}

main();
```

## Product roadmap..

