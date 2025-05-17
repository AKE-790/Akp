const axios = require('axios');
const apiKey = process.env.ON_DEMAND_API_KEY || 'YOUR_API_KEY_HERE';

async function createChatSession(externalUserId) {
  const url = 'https://api.on-demand.io/chat/v1/sessions';
  const headers = { apikey: apiKey };
  const body = { agentIds: [], externalUserId };
  try {
    const response = await axios.post(url, body, { headers });
    return response.data.data.id;
  } catch (e) {
    console.error('Failed to create chat session:', e.response?.data || e.message);
    throw e;
  }
}

async function submitQuery(sessionId, query) {
  const url = `https://api.on-demand.io/chat/v1/sessions/${sessionId}/query`;
  const headers = { apikey: apiKey };
  const body = {
    endpointId: 'predefined-openai-gpt4o',
    query,
    agentIds: ['agent-1712327325', 'agent-1713962163'],
    responseMode: 'sync',
    reasoningMode: 'medium'
  };
  try {
    const response = await axios.post(url, body, { headers });
    return response.data;
  } catch (e) {
    console.error('Failed to submit query:', e.response?.data || e.message);
    throw e;
  }
}

(async () => {
  try {
    const externalUserId = 'sample-user-123';
    const sessionId = await createChatSession(externalUserId);
    const result = await submitQuery(sessionId, 'Put your query here');
    console.log(result);
  } catch (e) {
    // Handle or log error as needed
  }
})();
