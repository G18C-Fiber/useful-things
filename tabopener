(async () => {
  const apiKey = 'AIzaSyA54JKMnNdmGQ2qnTK_mj44daAcMSEMmqY'; // Use your own
  const query = prompt('Enter a keyword or phrase:');
  if (!query) return;

  const promptText = `
You are a Google search assistant.

Your task is to generate 5 concise, high-quality Google search queries based on the following topic:

"${query}"

Requirements:
- Include the original topic itself as one of the queries.
- All outputs must be suitable for directly pasting into Google Search.
- Each query must be specific and distinct, but still clearly related to the topic.
- Do not include any numbers, bullets, or extra text.
- Output only the 5 search queries, one per line.

Begin:
`;

  const body = {
    contents: [
      {
        role: "user",
        parts: [{ text: promptText }]
      }
    ]
  };

  try {
    const res = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(body)
    });

    const data = await res.json();
    const rawText = data?.candidates?.[0]?.content?.parts?.map(p => p.text).join('\n');
    if (!rawText) {
      alert("No result returned from Gemini.");
      return;
    }

    const queries = rawText.split('\n')
      .map(q => q.trim())
      .filter(q => q.length > 0)
      .slice(0, 5);

    if (!queries.length) {
      alert("Couldn't extract search queries.");
      console.warn("Full response:", data);
      return;
    }

    for (const q of queries) {
      window.open(`https://www.google.com/search?q=${encodeURIComponent(q)}`, '_blank');
    }
  } catch (err) {
    alert('Failed to contact Gemini API. See console.');
    console.error(err);
  }
})();
