## Detecting LLM Vulnerabilities

1. Identify the LLM's inputs, including both direct (such as a prompt) and indirect (such as training data) inputs.
2. Work out what data and APIs the LLM has access to.
3. Probe this new attack surface for vulnerabilities.

## Exploiting LLM APIs

### Common Workflow

1. The client calls the LLM with the user's prompt.
2. The LLM detects that a function needs to be called and returns a JSON object containing arguments adhering to the external API's schema.
3. The client calls the function with the provided arguments.
4. The client processes the function's response.
5. The client calls the LLM again, appending the function response as a new message.
6. The LLM calls the external API with the function response.
7. The LLM summarizes the results of this API call back to the user.

### Mapping the Attack Surface

Ask the LLM directly which APIs it has access to, and ask more details about the interesting APIs.

If the LLM is not cooperative, trick it by providing misleading context. For instance claim to be the LLM's developer.

### Chaining Vulnerabilities

Keep in mind that a second order vulnerability can hide behind the LLM. For instance, an API that takes a file name as an input could be vulnerable to a path traversal attack.