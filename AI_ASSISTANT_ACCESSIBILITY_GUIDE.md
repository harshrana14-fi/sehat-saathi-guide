# AIAssistant Accessibility Implementation Guide

## Changes to Apply to src/components/AIAssistant.tsx

Due to technical limitations with automated edits, please apply these changes manually or use your IDE's find-and-replace feature.

### Change 1: Sidebar Navigation (Lines 267-272)

**Find:**
```typescript
      {/* Sidebar */}
      <div className="w-60 bg-secondary rounded-lg p-4 h-full mr-4 flex flex-col">
        <Button onClick={createNewChat} className="w-full mb-4 gap-2">
          <Plus className="w-4 h-4" />
          {language === 'hi' ? 'नई चैट' : 'New Chat'}
        </Button>
```

**Replace with:**
```typescript
      {/* Sidebar */}
      <nav 
        className="w-60 bg-secondary rounded-lg p-4 h-full mr-4 flex flex-col"
        aria-label={language === 'hi' ? 'चैट नेविगेशन' : 'Chat navigation'}
      >
        <Button 
          onClick={createNewChat} 
          className="w-full mb-4 gap-2"
          aria-label={language === 'hi' ? 'नई चैट बनाएं' : 'Create new chat'}
        >
          <Plus className="w-4 h-4" aria-hidden="true" />
          {language === 'hi' ? 'नई चैट' : 'New Chat'}
        </Button>
```

### Change 2: Chat History Heading (Lines 274-277)

**Find:**
```typescript
        <div className="flex-1 overflow-y-auto">
          <h3 className="text-sm font-semibold text-muted-foreground mb-2 px-2">
            {language === 'hi' ? 'चैट इतिहास' : 'Chat History'}
          </h3>
```

**Replace with:**
```typescript
        <div className="flex-1 overflow-y-auto">
          <h3 
            className="text-sm font-semibold text-muted-foreground mb-2 px-2"
            id="chat-history-heading"
          >
            {language === 'hi' ? 'चैट इतिहास' : 'Chat History'}
          </h3>
```

### Change 3: Chat History List (Lines 279-303)

**Find:**
```typescript
          <div className="space-y-1">
            {chatSessions.map((chat) => (
              <Card 
                key={chat.id}
                className={`cursor-pointer transition-colors ${activeChatId === chat.id ? 'bg-primary text-primary-foreground' : 'hover:bg-muted'}`}
                onClick={() => switchToChat(chat.id)}
              >
                <CardContent className="p-3 flex items-center justify-between">
                  <div className="flex items-center gap-2 overflow-hidden">
                    <MessageCircle className="w-4 h-4 flex-shrink-0" />
                    <span className="truncate text-sm">
                      {chat.title}
                    </span>
                  </div>
                  <Button
                    variant="ghost"
                    size="sm"
                    className="p-1 h-auto text-muted-foreground hover:text-destructive"
                    onClick={(e) => deleteChat(chat.id, e)}
                  >
                    <Trash2 className="w-4 h-4" />
                  </Button>
                </CardContent>
              </Card>
            ))}
          </div>
```

**Replace with:**
```typescript
          <div 
            className="space-y-1"
            role="list"
            aria-labelledby="chat-history-heading"
          >
            {chatSessions.map((chat) => (
              <Card 
                key={chat.id}
                className={`cursor-pointer transition-colors ${activeChatId === chat.id ? 'bg-primary text-primary-foreground' : 'hover:bg-muted'}`}
                onClick={() => switchToChat(chat.id)}
                role="listitem"
                tabIndex={0}
                onKeyDown={(e) => {
                  if (e.key === 'Enter' || e.key === ' ') {
                    e.preventDefault();
                    switchToChat(chat.id);
                  }
                }}
                aria-label={`${language === 'hi' ? 'चैट' : 'Chat'}: ${chat.title}${activeChatId === chat.id ? ` (${language === 'hi' ? 'सक्रिय' : 'active'})` : ''}`}
              >
                <CardContent className="p-3 flex items-center justify-between">
                  <div className="flex items-center gap-2 overflow-hidden">
                    <MessageCircle className="w-4 h-4 flex-shrink-0" aria-hidden="true" />
                    <span className="truncate text-sm">
                      {chat.title}
                    </span>
                  </div>
                  <Button
                    variant="ghost"
                    size="sm"
                    className="p-1 h-auto text-muted-foreground hover:text-destructive"
                    onClick={(e) => deleteChat(chat.id, e)}
                    aria-label={`${language === 'hi' ? 'हटाएं' : 'Delete'} ${chat.title}`}
                    title={`${language === 'hi' ? 'हटाएं' : 'Delete'} ${chat.title}`}
                  >
                    <Trash2 className="w-4 h-4" aria-hidden="true" />
                    <span className="sr-only">
                      {language === 'hi' ? `${chat.title} चैट हटाएं` : `Delete ${chat.title} chat`}
                    </span>
                  </Button>
                </CardContent>
              </Card>
            ))}
          </div>
```

### Change 4: Close Sidebar (Line 305-306)

**Find:**
```typescript
        </div>
      </div>
```

**Replace with:**
```typescript
        </div>
      </nav>
```

### Change 5: Main Chat Area (Lines 308-315)

**Find:**
```typescript
      {/* Main Chat Area */}
      <div className="flex-1 flex flex-col max-w-3xl mx-auto w-full">
        <div className="text-center mb-6">
          <h1 className="text-2xl font-bold text-foreground mb-2">{t.aiAssistant}</h1>
          <p className="text-muted-foreground">
            {language === 'hi' ? 'हमारे हैल्थ एआई सहायक के साथ बात करें' : 'Talk with our Health AI assistant'}
          </p>
        </div>
        
        {/* Chat Interface */}
        <Card className="flex-1 border-2 border-border shadow-lg flex flex-col">
          <CardHeader className="bg-primary text-primary-foreground">
            <CardTitle className="flex items-center gap-3">
              <MessageCircle className="w-6 h-6" />
```

**Replace with:**
```typescript
      {/* Main Chat Area */}
      <main className="flex-1 flex flex-col max-w-3xl mx-auto w-full">
        <div className="text-center mb-6">
          <h1 className="text-2xl font-bold text-foreground mb-2">{t.aiAssistant}</h1>
          <p className="text-muted-foreground">
            {language === 'hi' ? 'हमारे हैल्थ एआई सहायक के साथ बात करें' : 'Talk with our Health AI assistant'}
          </p>
        </div>
        
        {/* Chat Interface */}
        <Card className="flex-1 border-2 border-border shadow-lg flex flex-col" role="region" aria-label={t.aiAssistant}>
          <CardHeader className="bg-primary text-primary-foreground">
            <CardTitle className="flex items-center gap-3">
              <MessageCircle className="w-6 h-6" aria-hidden="true" />
```

### Change 6: Chat Messages Area (Lines 326-340)

**Find:**
```typescript
          <ScrollArea className="flex-1 p-4">
            <div className="space-y-4">
              {messages.map((message) => (
                <div key={message.id} className={`flex gap-3 ${message.role === 'user' ? 'flex-row-reverse' : ''}`}>
                  <div className="w-8 h-8 rounded-full flex items-center justify-center bg-primary text-primary-foreground">
                    {message.role === 'user' ? <User className="w-4 h-4" /> : <Bot className="w-4 h-4" />}
                  </div>
                  <div className="max-w-[80%] p-3 rounded-lg bg-secondary">
                    <p className="text-sm whitespace-pre-wrap">{message.content}</p>
                  </div>
                </div>
              ))}

              {isTyping && <p className="text-sm text-muted-foreground">AI is typing…</p>}
              <div ref={scrollRef} />
            </div>
          </ScrollArea>
```

**Replace with:**
```typescript
          <ScrollArea className="flex-1 p-4">
            <div 
              className="space-y-4"
              role="log"
              aria-live="polite"
              aria-atomic="false"
              aria-label={language === 'hi' ? 'चैट संदेश' : 'Chat messages'}
            >
              {messages.map((message) => (
                <div 
                  key={message.id} 
                  className={`flex gap-3 ${message.role === 'user' ? 'flex-row-reverse' : ''}`}
                  role="article"
                  aria-label={`${message.role === 'user' ? (language === 'hi' ? 'आप' : 'You') : (language === 'hi' ? 'AI सहायक' : 'AI Assistant')}: ${message.content}`}
                >
                  <div 
                    className="w-8 h-8 rounded-full flex items-center justify-center bg-primary text-primary-foreground"
                    aria-hidden="true"
                  >
                    {message.role === 'user' ? <User className="w-4 h-4" /> : <Bot className="w-4 h-4" />}
                  </div>
                  <div className="max-w-[80%] p-3 rounded-lg bg-secondary">
                    <p className="text-sm whitespace-pre-wrap">{message.content}</p>
                  </div>
                </div>
              ))}

              {isTyping && (
                <p 
                  className="text-sm text-muted-foreground"
                  role="status"
                  aria-live="polite"
                  aria-label={language === 'hi' ? 'AI टाइप कर रहा है' : 'AI is typing'}
                >
                  AI is typing…
                </p>
              )}
              <div ref={scrollRef} />
            </div>
          </ScrollArea>
```

### Change 7: Message Input Form (Lines 344-357)

**Find:**
```typescript
          <CardContent className="border-t-2 border-border p-4">
            <div className="flex gap-2">
              <Input
                value={input}
                onChange={(e) => setInput(e.target.value)}
                onKeyDown={handleKeyDown}
                placeholder={t.askHealth}
                disabled={loading}
              />
              <Button onClick={handleSend} disabled={loading}>
                <Send className="w-4 h-4" />
              </Button>
            </div>
          </CardContent>
        </Card>
      </div>
```

**Replace with:**
```typescript
          <CardContent className="border-t-2 border-border p-4">
            <form 
              onSubmit={(e) => {
                e.preventDefault();
                handleSend();
              }}
              aria-label={language === 'hi' ? 'संदेश भेजें' : 'Send message'}
            >
              <div className="flex gap-2">
                <Input
                  value={input}
                  onChange={(e) => setInput(e.target.value)}
                  onKeyDown={handleKeyDown}
                  placeholder={t.askHealth}
                  disabled={loading}
                  aria-label={t.askHealth}
                  aria-describedby="chat-input-description"
                  id="chat-message-input"
                />
                <span id="chat-input-description" className="sr-only">
                  {language === 'hi' 
                    ? 'अपना स्वास्थ्य प्रश्न टाइप करें और भेजने के लिए Enter दबाएं'
                    : 'Type your health question and press Enter to send'
                  }
                </span>
                <Button 
                  onClick={handleSend} 
                  disabled={loading}
                  aria-label={language === 'hi' ? 'संदेश भेजें' : 'Send message'}
                  type="submit"
                >
                  <Send className="w-4 h-4" aria-hidden="true" />
                  <span className="sr-only">
                    {language === 'hi' ? 'संदेश भेजें' : 'Send message'}
                  </span>
                </Button>
              </div>
            </form>
          </CardContent>
        </Card>
      </main>
```

## Summary of Changes

1. ✅ Changed sidebar `<div>` to semantic `<nav>` element
2. ✅ Added `aria-label` to navigation region
3. ✅ Added descriptive `aria-label` to "New Chat" button
4. ✅ Marked decorative icons with `aria-hidden="true"`
5. ✅ Added `id` to chat history heading for labeling
6. ✅ Added `role="list"` and `aria-labelledby` to chat history container
7. ✅ Added `role="listitem"` to each chat card
8. ✅ Added keyboard navigation (Enter/Space) to chat cards
9. ✅ Added descriptive `aria-label` to chat cards with active state
10. ✅ Added descriptive labels to delete buttons with chat title
11. ✅ Added `sr-only` spans for additional screen reader context
12. ✅ Changed main area `<div>` to semantic `<main>` element
13. ✅ Added `role="region"` and `aria-label` to chat interface card
14. ✅ Added `role="log"` and `aria-live="polite"` to messages container
15. ✅ Added `role="article"` and descriptive labels to each message
16. ✅ Added `role="status"` and `aria-live` to typing indicator
17. ✅ Wrapped input in semantic `<form>` element
18. ✅ Added `aria-label` and `aria-describedby` to message input
19. ✅ Added hidden instructions for screen readers
20. ✅ Added descriptive label to send button

All changes support both Hindi and English languages!
