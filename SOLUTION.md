# Solution

## TODO 1 — `ask_question()`

1. Search the FAISS vector store for the 3 chunks most similar to the question:
   `vector_store.similarity_search(question, k=3)`.
2. Pull the text out of each result (`doc.page_content`) — these become the `sources`.
3. Join the source texts with blank lines to build a single `context` string.
4. Drop `context` and `question` into `PROMPT_TEMPLATE` to get the final prompt.
5. Send the prompt to the local LLM: `llm(prompt)`, then read the answer out of
   `result[0]["generated_text"]`.
6. Return `{"answer": answer, "sources": sources}`.

So the flow is: **question → find relevant chunks → build a prompt with those chunks →
ask the LLM → return the answer plus the chunks it was based on.**

## TODO 2 — `main()`

1. Build the vector store from the `data/` folder with `build_knowledge_base(data_dir)`.
2. Load the local model with `get_llm()`.
3. Loop forever:
   - Read a question with `input("> ")`.
   - If the user typed `quit`, stop the loop.
   - If they typed nothing, ask again.
   - Otherwise call `ask_question(vector_store, llm, question)`.
   - Print the retrieved sources, then print the answer.

## Result

Both pieces just wire together the parts that were already built for us
(`knowledge_base.py` for retrieval, `get_llm()` for generation) into a simple
retrieve → prompt → generate loop. All 10 tests in `tests/test_pipeline.py` pass.
