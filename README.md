// Single-file Next.js 14 AI Chatbot with App Router
// Save this as app/(chat)/page.tsx (requires API route and dependencies as described previously)

'use client';

import { useState } from 'react';

interface Message {
  role: 'user' | 'assistant';
  content: string;
}

export default function ChatPage() {
  const [messages, setMessages] = useState<Message[]>([]);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);

  const sendMessage = async () => {
    if (!input.trim()) return;

    const userMessage: Message = { role: 'user', content: input };
    setMessages((prev) => [...prev, userMessage]);
    setInput('');
    setLoading(true);

    try {
      const response = await fetch('/api/chat', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ messages: [...messages, userMessage] }),
      });

      const data = await response.json();
      const assistantMessage: Message = { role: 'assistant', content: data.reply };
      setMessages((prev) => [...prev, assistantMessage]);
    } catch (error) {
      console.error('Error:', error);
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="flex flex-col h-screen p-4 bg-gray-100">
      <h1 className="text-2xl font-bold mb-4 text-center">AI Chatbot</h1>
      <div className="flex-1 overflow-y-auto mb-4 border rounded p-4 bg-white">
        {messages.map((msg, index) => (
          <div key={index} className={`mb-2 ${msg.role === 'user' ? 'text-right' : 'text-left'}`}>
            <span className={`inline-block p-2 rounded max-w-xs break-words ${msg.role === 'user' ? 'bg-blue-500 text-white' : 'bg-gray-200'}`}>
              {msg.content}
            </span>
          </div>
        ))}
        {loading && <div className="text-center text-gray-500">AI is thinking...</div>}
      </div>
      <div className="flex">
        <input
          type="text"
          value={input}
          onChange={(e) => setInput(e.target.value)}
          onKeyPress={(e) => e.key === 'Enter' && sendMessage()}
          className="flex-1 p-2 border rounded-l focus:outline-none focus:ring-2 focus:ring-blue-500"
          placeholder="Type your message..."
          disabled={loading}
        />
        <button 
          onClick={sendMessage} 
          className="p-2 bg-blue-500 text-white rounded-r hover:bg-blue-600 disabled:opacity-50" 
          disabled={loading || !input.trim()}
        >
          Send
        </button>
      </div>
    </div>
  );
}

// For the API route (app/api/chat/route.ts):
/*
import { NextRequest, NextResponse } from 'next/server';
import OpenAI from 'openai';

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
});

export async function POST(request: NextRequest) {
  try {
    const { messages } = await request.json();
    const completion = await openai.chat.completions.create({
      model: 'gpt-3.5-turbo',
      messages,
    });
    const reply = completion.choices[0].message.content;
    return NextResponse.json({ reply });
  } catch (error) {
    console.error(error);
    return NextResponse.json({ error: 'Failed to generate response' }, { status: 500 });
  }
}
*/

// For layout (app/(chat)/layout.tsx):
/*
import { Metadata } from 'next';

export const metadata: Metadata = {
  title: 'AI Chatbot',
  description: 'Next.js 14 and App Router-ready AI chatbot.',
  openGraph: {
    images: ['/opengraph-image.png'],
  },
};

export default function ChatLayout({ children }: { children: React.ReactNode }) {
  return <>{children}</>;
}
*/

// For opengraph image (app/(chat)/opengraph-image.tsx):
/*
import { ImageResponse } from 'next/og';

export const size = { width: 1200, height: 630 };
export const contentType = 'image/png';

export default function OpenGraphImage() {
  return new ImageResponse(
    (
      <div style={{ fontSize: 48, background: 'white', width: '100%', height: '100%', display: 'flex', alignItems: 'center', justifyContent: 'center' }}>
        Next.js 14 and App Router-ready AI Chatbot
      </div>
    ),
    { ...size }
  );
}
*/
