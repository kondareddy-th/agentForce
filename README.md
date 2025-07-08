<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BNY Agent Builder Platform</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        :root {
            --primary-navy: #003865;
            --primary-navy-dark: #002a4e;
            --primary-navy-light: #004d85;
            --accent-teal: #00a6a0;
            --text-primary: #1a1a1a;
            --text-secondary: #6b7280;
            --border-color: #e5e7eb;
            --background: #f9fafb;
            --white: #ffffff;
            --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
            --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
            --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
            --shadow-xl: 0 20px 25px -5px rgba(0, 0, 0, 0.1);
            --transition-smooth: cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background-color: var(--background);
            display: flex;
            height: 100vh;
            overflow: hidden;
            color: var(--text-primary);
        }
        
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        
        /* Sidebar */
        .sidebar {
            width: 64px;
            background-color: var(--white);
            border-right: 1px solid var(--border-color);
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 24px 0;
            gap: 20px;
            position: relative;
            z-index: 100;
        }
        
        .sidebar-icon {
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            border-radius: 10px;
            transition: all 0.2s var(--transition-smooth);
            position: relative;
        }
        
        .sidebar-icon:hover {
            background-color: rgba(0, 56, 101, 0.08);
            transform: translateY(-1px);
        }
        
        .sidebar-icon.active {
            background-color: rgba(0, 56, 101, 0.12);
        }
        
        .sidebar-icon.active::before {
            content: '';
            position: absolute;
            left: -24px;
            top: 50%;
            transform: translateY(-50%);
            width: 4px;
            height: 24px;
            background: var(--primary-navy);
            border-radius: 0 4px 4px 0;
        }
        
        .sidebar-icon svg {
            width: 22px;
            height: 22px;
            stroke: var(--text-secondary);
            fill: none;
            stroke-width: 2;
            transition: stroke 0.2s;
        }
        
        .sidebar-icon:hover svg,
        .sidebar-icon.active svg {
            stroke: var(--primary-navy);
        }
        
        .sidebar-icon:last-child {
            background: linear-gradient(135deg, var(--primary-navy) 0%, var(--primary-navy-light) 100%);
            color: white;
            font-family: 'Inter', sans-serif;
        }
        
        .sidebar-icon:last-child:hover {
            transform: scale(1.05);
            box-shadow: 0 4px 12px rgba(0, 56, 101, 0.3);
        }
        
        /* New Session Button */
        .new-session-btn {
            background: linear-gradient(135deg, var(--primary-navy) 0%, var(--primary-navy-dark) 100%);
            border-radius: 12px;
            color: white;
            font-size: 24px;
            font-weight: 300;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s var(--transition-smooth);
            box-shadow: var(--shadow-md);
            position: relative;
            overflow: hidden;
        }
        
        .new-session-btn::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 0;
            height: 0;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.2);
            transform: translate(-50%, -50%);
            transition: width 0.6s, height 0.6s;
        }
        
        .new-session-btn:hover::before {
            width: 80px;
            height: 80px;
        }
        
        .new-session-btn:hover {
            transform: scale(1.05);
            box-shadow: 0 6px 20px rgba(0, 56, 101, 0.3);
        }
        
        /* Chat History Dropdown */
        .chat-history-dropdown {
            position: absolute;
            top: 0;
            left: calc(100% + 12px);
            background: var(--white);
            border-radius: 16px;
            box-shadow: var(--shadow-xl), 0 0 0 1px rgba(0, 0, 0, 0.05);
            min-width: 300px;
            max-height: 500px;
            overflow-y: auto;
            opacity: 0;
            visibility: hidden;
            transform: translateX(-10px) scale(0.95);
            transition: all 0.3s var(--transition-smooth);
            backdrop-filter: blur(10px);
            z-index: 1000;
        }
        
        .chat-history-dropdown.active {
            opacity: 1;
            visibility: visible;
            transform: translateX(0) scale(1);
        }
        
        .chat-history-header {
            padding: 16px 20px;
            border-bottom: 1px solid var(--border-color);
            font-weight: 600;
            color: var(--text-primary);
            position: sticky;
            top: 0;
            background: var(--white);
            z-index: 1;
        }
        
        .chat-item {
            padding: 14px 20px;
            cursor: pointer;
            transition: all 0.2s ease;
            display: flex;
            flex-direction: column;
            gap: 6px;
            border-bottom: 1px solid var(--background);
            position: relative;
            overflow: hidden;
        }
        
        .chat-item::before {
            content: '';
            position: absolute;
            left: 0;
            top: 0;
            height: 100%;
            width: 3px;
            background: var(--primary-navy);
            transform: translateX(-3px);
            transition: transform 0.2s;
        }
        
        .chat-item:hover {
            background-color: rgba(0, 56, 101, 0.05);
        }
        
        .chat-item:hover::before {
            transform: translateX(0);
        }
        
        .chat-title {
            font-size: 14px;
            font-weight: 600;
            color: var(--text-primary);
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        
        .chat-preview {
            font-size: 13px;
            color: var(--text-secondary);
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        
        .chat-date {
            font-size: 11px;
            color: var(--text-secondary);
        }
        
        /* Options Dropdown (next to web search) */
        .options-dropdown {
            position: absolute;
            bottom: calc(100% + 8px);
            left: 0;
            background: var(--white);
            border-radius: 12px;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.15);
            min-width: 220px;
            opacity: 0;
            visibility: hidden;
            transform: translateY(10px);
            transition: all 0.3s ease;
            z-index: 2000;
            border: 1px solid var(--border-color);
        }
        
        .options-dropdown.open {
            opacity: 1;
            visibility: visible;
            transform: translateY(0);
        }
        
        .option-item {
            padding: 12px 16px;
            cursor: pointer;
            transition: all 0.2s;
            font-size: 14px;
            color: var(--text-primary);
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .option-item:hover {
            background: rgba(0, 56, 101, 0.05);
            color: var(--primary-navy);
        }
        
        .option-item svg {
            flex-shrink: 0;
        }
        
        .option-item:first-child {
            border-radius: 12px 12px 0 0;
        }
        
        .option-item:last-child {
            border-radius: 0 0 12px 12px;
        }
        
        /* Main Content */
        .main-content {
            flex: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            overflow-y: auto;
            position: relative;
        }
        
        .main-content.initial {
            padding: 48px 24px;
        }
        
        .main-content.conversation {
            padding: 0;
        }
        
        /* Page transitions */
        .page {
            width: 100%;
            display: none;
            animation: fadeIn 0.4s ease;
        }
        
        .page.active {
            display: flex;
            flex-direction: column;
            align-items: center;
            height: 100%;
        }
        
        @keyframes fadeIn {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        .greeting {
            display: flex;
            align-items: center;
            gap: 16px;
            font-size: 40px;
            color: var(--text-primary);
            margin-bottom: 48px;
            font-weight: 300;
            letter-spacing: -0.02em;
        }
        
        .bny-logo {
            font-size: 36px;
            font-weight: 700;
            color: var(--primary-navy);
            letter-spacing: -0.05em;
        }
        
        /* Mode Selector Tabs */
        .mode-selector {
            display: flex;
            gap: 4px;
            margin-bottom: 24px;
            background: rgba(255, 255, 255, 0.8);
            padding: 4px;
            border-radius: 14px;
            box-shadow: var(--shadow-sm), inset 0 1px 2px rgba(0, 0, 0, 0.05);
            backdrop-filter: blur(10px);
        }
        
        .mode-tab {
            padding: 12px 28px;
            border: none;
            background: transparent;
            border-radius: 10px;
            cursor: pointer;
            font-size: 14px;
            font-weight: 500;
            color: var(--text-secondary);
            transition: all 0.3s var(--transition-smooth);
            position: relative;
        }
        
        .mode-tab:hover {
            color: var(--text-primary);
        }
        
        .mode-tab.active {
            background: var(--white);
            color: var(--primary-navy);
            box-shadow: var(--shadow-md);
            font-weight: 600;
        }
        
        /* Input Container */
        .input-container {
            width: 100%;
            max-width: 720px;
            background: var(--white);
            border-radius: 20px;
            box-shadow: var(--shadow-md);
            padding: 20px 24px 16px;
            margin-bottom: 36px;
            transition: all 0.3s var(--transition-smooth);
            border: 1px solid transparent;
            position: relative;
            z-index: 10;
        }
        
        .input-container:hover {
            box-shadow: var(--shadow-lg);
            border-color: var(--border-color);
        }
        
        .input-container:focus-within {
            border-color: var(--primary-navy);
            box-shadow: 0 0 0 3px rgba(0, 56, 101, 0.1), var(--shadow-lg);
        }
        
        .input-wrapper {
            position: relative;
        }
        
        .input-field {
            width: 100%;
            border: none;
            outline: none;
            font-size: 16px;
            color: var(--text-primary);
            padding: 12px 40px 12px 0;
            font-weight: 400;
            background: transparent;
            resize: none;
            overflow-y: auto;
            line-height: 1.5;
            font-family: inherit;
            min-height: 24px;
            max-height: 120px;
            transition: height 0.2s ease;
        }
        
        .input-field::placeholder {
            color: var(--text-secondary);
        }
        
        .input-field::-webkit-scrollbar {
            width: 6px;
        }
        
        .input-field::-webkit-scrollbar-track {
            background: var(--background);
            border-radius: 3px;
        }
        
        .input-field::-webkit-scrollbar-thumb {
            background: var(--border-color);
            border-radius: 3px;
        }
        
        .input-field::-webkit-scrollbar-thumb:hover {
            background: var(--text-secondary);
        }
        
        .char-count {
            position: absolute;
            right: 12px;
            bottom: 12px;
            font-size: 11px;
            color: var(--text-secondary);
            opacity: 0;
            transition: opacity 0.2s;
        }
        
        .input-wrapper:focus-within .char-count {
            opacity: 1;
        }
        
        /* Input Actions and Attachments Container */
        .actions-attachments-wrapper {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }
        
        .input-actions {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding-top: 16px;
            border-top: 1px solid var(--border-color);
        }
        
        .left-actions {
            display: flex;
            gap: 12px;
            align-items: center;
            position: relative;
        }
        
        .action-btn {
            background: var(--white);
            border: 1px solid var(--border-color);
            cursor: pointer;
            padding: 10px;
            border-radius: 10px;
            transition: all 0.2s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            width: 40px;
            height: 40px;
        }
        
        .action-btn:hover {
            background-color: var(--background);
            border-color: var(--primary-navy);
            transform: translateY(-1px);
        }
        
        .action-btn svg {
            width: 20px;
            height: 20px;
            stroke: var(--text-secondary);
            transition: stroke 0.2s;
        }
        
        .action-btn:hover svg {
            stroke: var(--primary-navy);
        }
        
        /* File Attachments */
        .attachments-container {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            padding: 0 4px;
            max-height: 120px;
            overflow-y: auto;
            empty-cells: hide;
        }
        
        .attachments-container:not(:empty) {
            padding-top: 12px;
            border-top: 1px solid var(--border-color);
        }
        
        .attachment-item {
            display: flex;
            align-items: center;
            gap: 6px;
            padding: 6px 10px;
            background: var(--background);
            border: 1px solid var(--border-color);
            border-radius: 8px;
            font-size: 13px;
            color: var(--text-primary);
            position: relative;
            transition: all 0.2s;
            max-width: 200px;
            animation: attachmentIn 0.3s ease;
        }
        
        @keyframes attachmentIn {
            from {
                opacity: 0;
                transform: scale(0.9);
            }
            to {
                opacity: 1;
                transform: scale(1);
            }
        }
        
        .attachment-item:hover {
            border-color: var(--primary-navy);
            background: rgba(0, 56, 101, 0.05);
            transform: translateY(-1px);
        }
        
        .attachment-icon {
            width: 16px;
            height: 16px;
            stroke: var(--primary-navy);
            flex-shrink: 0;
        }
        
        .attachment-name {
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        
        .attachment-size {
            font-size: 11px;
            color: var(--text-secondary);
            margin-left: 4px;
        }
        
        .attachment-remove {
            position: absolute;
            top: -6px;
            right: -6px;
            width: 20px;
            height: 20px;
            background: var(--primary-navy);
            color: white;
            border-radius: 50%;
            display: none;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-size: 12px;
            transition: all 0.2s;
            box-shadow: var(--shadow-sm);
        }
        
        .attachment-item:hover .attachment-remove {
            display: flex;
        }
        
        .attachment-remove:hover {
            transform: scale(1.1);
            background: var(--primary-navy-dark);
        }
        
        /* Web Search Button */
        .web-search-btn {
            display: flex;
            align-items: center;
            gap: 8px;
            padding: 10px 18px;
            background: var(--white);
            border: 1px solid var(--border-color);
            border-radius: 24px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 14px;
            color: var(--text-secondary);
            font-weight: 500;
            height: 40px;
        }
        
        .web-search-btn:hover {
            border-color: var(--accent-teal);
            color: var(--accent-teal);
            background: rgba(0, 166, 160, 0.05);
            transform: translateY(-1px);
        }
        
        .web-search-btn.active {
            background: rgba(0, 166, 160, 0.1);
            border-color: var(--accent-teal);
            color: var(--accent-teal);
            box-shadow: 0 2px 8px rgba(0, 166, 160, 0.2);
        }
        
        /* Agent Selector Dropdown */
        .agent-dropdown-container {
            position: relative;
        }
        
        .agent-selector {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 10px 18px;
            background: var(--white);
            border: 1px solid var(--border-color);
            border-radius: 24px;
            cursor: pointer;
            color: var(--text-primary);
            font-size: 14px;
            font-weight: 500;
            transition: all 0.2s ease;
            min-width: 180px;
            height: 40px;
        }
        
        .agent-selector:hover {
            border-color: var(--primary-navy);
            background: rgba(0, 56, 101, 0.05);
        }
        
        .agent-selector .agent-name {
            flex: 1;
            text-align: left;
        }
        
        .agent-selector .dropdown-arrow {
            width: 16px;
            height: 16px;
            transition: transform 0.2s;
        }
        
        .agent-selector.open .dropdown-arrow {
            transform: rotate(180deg);
        }
        
        .agent-dropdown {
            position: absolute;
            top: calc(100% + 8px);
            right: 0;
            background: var(--white);
            border-radius: 12px;
            box-shadow: var(--shadow-xl);
            min-width: 280px;
            max-height: 300px;
            overflow-y: auto;
            opacity: 0;
            visibility: hidden;
            transform: translateY(-10px);
            transition: all 0.3s ease;
            z-index: 2000;
            border: 1px solid var(--border-color);
        }
        
        .agent-dropdown.open {
            opacity: 1;
            visibility: visible;
            transform: translateY(0);
        }
        
        .agent-option {
            padding: 12px 16px;
            cursor: pointer;
            transition: all 0.2s;
            border-bottom: 1px solid var(--background);
        }
        
        .agent-option:hover {
            background: rgba(0, 56, 101, 0.05);
        }
        
        .agent-option:first-child {
            border-radius: 12px 12px 0 0;
        }
        
        .agent-option:last-child {
            border-radius: 0 0 12px 12px;
            border-bottom: none;
        }
        
        .agent-option.selected {
            background: rgba(0, 56, 101, 0.08);
        }
        
        .agent-option-content {
            display: flex;
            flex-direction: column;
            gap: 4px;
        }
        
        .agent-option-name {
            font-size: 14px;
            font-weight: 600;
            color: var(--text-primary);
        }
        
        .agent-option-desc {
            font-size: 12px;
            color: var(--text-secondary);
        }
        
        .send-btn {
            background: linear-gradient(135deg, var(--primary-navy) 0%, var(--primary-navy-dark) 100%);
            color: white;
            width: 40px;
            height: 40px;
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            border: none;
            transition: all 0.3s ease;
            box-shadow: var(--shadow-md);
            font-size: 18px;
        }
        
        .send-btn:hover {
            transform: translateY(-2px);
            box-shadow: var(--shadow-lg);
        }
        
        .send-btn:active {
            transform: scale(0.95);
        }
        
        /* Flow View */
        .back-to-home {
            position: absolute;
            top: 20px;
            left: 20px;
            display: flex;
            align-items: center;
            gap: 8px;
            padding: 8px 16px;
            background: var(--white);
            border: 1px solid var(--border-color);
            border-radius: 10px;
            cursor: pointer;
            font-size: 14px;
            font-weight: 500;
            color: var(--text-secondary);
            transition: all 0.2s;
            z-index: 10;
        }
        
        .back-to-home:hover {
            border-color: var(--primary-navy);
            color: var(--primary-navy);
            transform: translateX(-4px);
        }
        
        .flow-container {
            flex: 1;
            width: 100%;
            display: flex;
            flex-direction: column;
            overflow-y: auto;
            padding: 40px 24px;
            max-width: 1200px;
            margin: 0 auto;
        }
        
        .flow-step {
            margin-bottom: 32px;
            animation: fadeIn 0.4s ease;
        }
        
        .step-header {
            display: flex;
            align-items: center;
            gap: 12px;
            margin-bottom: 16px;
        }
        
        .step-number {
            width: 32px;
            height: 32px;
            background: var(--primary-navy);
            color: white;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 600;
            font-size: 14px;
        }
        
        .step-title {
            font-size: 18px;
            font-weight: 600;
            color: var(--text-primary);
        }
        
        .step-content {
            margin-left: 44px;
            background: var(--white);
            border-radius: 16px;
            padding: 24px;
            box-shadow: var(--shadow-sm);
            border: 1px solid var(--border-color);
        }
        
        .query-card {
            background: rgba(0, 56, 101, 0.05);
            border: 1px solid var(--primary-navy);
            border-radius: 12px;
            padding: 16px;
            margin-bottom: 20px;
        }
        
        .query-label {
            font-size: 12px;
            font-weight: 600;
            color: var(--primary-navy);
            text-transform: uppercase;
            letter-spacing: 0.05em;
            margin-bottom: 8px;
        }
        
        .query-text {
            font-size: 16px;
            color: var(--text-primary);
            line-height: 1.6;
        }
        
        .result-card {
            background: var(--background);
            border-radius: 12px;
            padding: 20px;
            position: relative;
            overflow: hidden;
        }
        
        .result-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 4px;
            height: 100%;
            background: linear-gradient(180deg, var(--accent-teal) 0%, var(--primary-navy) 100%);
        }
        
        .result-section {
            margin-bottom: 24px;
        }
        
        .result-section:last-child {
            margin-bottom: 0;
        }
        
        .result-section h4 {
            font-size: 16px;
            font-weight: 600;
            color: var(--text-primary);
            margin-bottom: 12px;
        }
        
        .result-list {
            list-style: none;
            padding: 0;
        }
        
        .result-list li {
            padding: 8px 0;
            padding-left: 20px;
            position: relative;
            color: var(--text-secondary);
        }
        
        .result-list li::before {
            content: '•';
            position: absolute;
            left: 0;
            color: var(--accent-teal);
            font-weight: bold;
        }
        
        .action-buttons {
            display: flex;
            gap: 12px;
            margin-top: 20px;
        }
        
        .action-button {
            padding: 10px 20px;
            border-radius: 10px;
            border: none;
            font-size: 14px;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .action-button.primary {
            background: var(--primary-navy);
            color: white;
        }
        
        .action-button.primary:hover {
            background: var(--primary-navy-dark);
            transform: translateY(-1px);
            box-shadow: var(--shadow-md);
        }
        
        .action-button.secondary {
            background: var(--white);
            color: var(--text-primary);
            border: 1px solid var(--border-color);
        }
        
        .action-button.secondary:hover {
            border-color: var(--primary-navy);
            color: var(--primary-navy);
            background: rgba(0, 56, 101, 0.05);
        }
        
        .progress-indicator {
            display: flex;
            align-items: center;
            gap: 8px;
            margin: 20px 0;
            padding: 12px 16px;
            background: rgba(0, 166, 160, 0.1);
            border-radius: 8px;
            color: var(--accent-teal);
            font-size: 14px;
            font-weight: 500;
        }
        
        .spinner {
            width: 16px;
            height: 16px;
            border: 2px solid transparent;
            border-top-color: currentColor;
            border-radius: 50%;
            animation: spin 0.8s linear infinite;
        }
        
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        
        .agent-preview {
            background: var(--white);
            border: 2px solid var(--border-color);
            border-radius: 16px;
            padding: 24px;
            margin-top: 24px;
        }
        
        .agent-preview-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 20px;
        }
        
        .agent-preview-title {
            font-size: 20px;
            font-weight: 600;
            color: var(--text-primary);
        }
        
        .agent-status {
            display: flex;
            align-items: center;
            gap: 8px;
            padding: 6px 12px;
            background: rgba(16, 185, 129, 0.1);
            color: #10b981;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
        }
        
        .agent-status::before {
            content: '';
            width: 8px;
            height: 8px;
            background: currentColor;
            border-radius: 50%;
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.5; }
            100% { opacity: 1; }
        }
        
        /* Bottom Search Bar */
        .bottom-search-container {
            position: sticky;
            bottom: 0;
            width: 100%;
            background: var(--background);
            border-top: 1px solid var(--border-color);
            padding: 20px;
            box-shadow: 0 -4px 20px rgba(0, 0, 0, 0.05);
        }
        
        .bottom-search-wrapper {
            max-width: 900px;
            margin: 0 auto;
        }
        
        /* Quick Actions */
        .quick-actions {
            display: flex;
            gap: 16px;
            margin-bottom: 48px;
            flex-wrap: wrap;
            justify-content: center;
            position: relative;
            z-index: 1;
        }
        
        .quick-action {
            display: flex;
            align-items: center;
            gap: 12px;
            padding: 14px 28px;
            background: var(--white);
            border: 2px solid var(--border-color);
            border-radius: 28px;
            cursor: pointer;
            transition: all 0.3s var(--transition-smooth);
            font-size: 14px;
            color: var(--text-primary);
            font-weight: 500;
            position: relative;
            overflow: hidden;
        }
        
        .quick-action::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, rgba(0, 56, 101, 0.1) 0%, rgba(0, 56, 101, 0.05) 100%);
            transition: left 0.3s ease;
            z-index: -1;
        }
        
        .quick-action:hover {
            border-color: var(--primary-navy);
            color: var(--primary-navy);
            transform: translateY(-2px);
            box-shadow: var(--shadow-md);
        }
        
        .quick-action:hover::before {
            left: 0;
        }
        
        .quick-action svg {
            width: 20px;
            height: 20px;
            stroke: currentColor;
            transition: all 0.3s ease;
        }
        
        /* Project Cards Section */
        .projects-section {
            width: 100%;
            max-width: 920px;
            margin-top: 48px;
        }
        
        .section-title {
            font-size: 24px;
            color: var(--text-primary);
            margin-bottom: 32px;
            font-weight: 600;
            text-align: center;
            letter-spacing: -0.02em;
        }
        
        .project-cards {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
            gap: 24px;
        }
        
        .project-card {
            background: var(--white);
            border: 2px solid var(--border-color);
            border-radius: 20px;
            padding: 36px;
            cursor: pointer;
            transition: all 0.3s var(--transition-smooth);
            position: relative;
            overflow: hidden;
        }
        
        .project-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 4px;
            background: linear-gradient(90deg, var(--primary-navy) 0%, var(--primary-navy-light) 100%);
            transform: scaleX(0);
            transform-origin: left;
            transition: transform 0.3s ease;
        }
        
        .project-card:hover {
            border-color: var(--primary-navy);
            box-shadow: var(--shadow-lg);
            transform: translateY(-4px);
        }
        
        .project-card:hover::before {
            transform: scaleX(1);
        }
        
        .project-icon {
            width: 64px;
            height: 64px;
            margin-bottom: 24px;
            display: flex;
            align-items: center;
            justify-content: center;
            background: linear-gradient(135deg, rgba(0, 56, 101, 0.1) 0%, rgba(0, 56, 101, 0.05) 100%);
            border-radius: 16px;
            transition: all 0.3s ease;
        }
        
        .project-card:hover .project-icon {
            transform: scale(1.1) rotate(5deg);
            background: linear-gradient(135deg, rgba(0, 56, 101, 0.15) 0%, rgba(0, 56, 101, 0.1) 100%);
        }
        
        .project-icon svg {
            width: 36px;
            height: 36px;
            stroke: var(--primary-navy);
            stroke-width: 1.5;
        }
        
        .project-title {
            font-size: 20px;
            font-weight: 600;
            color: var(--text-primary);
            margin-bottom: 12px;
            letter-spacing: -0.01em;
        }
        
        .project-description {
            font-size: 15px;
            color: var(--text-secondary);
            line-height: 1.6;
        }
        
        /* Modal Styles */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.5);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 3000;
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s ease;
            backdrop-filter: blur(5px);
        }
        
        .modal-overlay.active {
            opacity: 1;
            visibility: visible;
        }
        
        .modal {
            background: var(--white);
            border-radius: 24px;
            padding: 36px;
            max-width: 600px;
            width: 90%;
            max-height: 80vh;
            overflow: hidden;
            display: flex;
            flex-direction: column;
            transform: scale(0.9) translateY(20px);
            transition: all 0.3s var(--transition-smooth);
            box-shadow: var(--shadow-xl);
        }
        
        .modal-overlay.active .modal {
            transform: scale(1) translateY(0);
        }
        
        .modal-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 24px;
        }
        
        .modal-title {
            font-size: 24px;
            font-weight: 600;
            color: var(--text-primary);
        }
        
        .close-modal {
            width: 32px;
            height: 32px;
            border-radius: 8px;
            border: none;
            background: var(--background);
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.2s;
        }
        
        .close-modal:hover {
            background: rgba(0, 56, 101, 0.1);
            transform: rotate(90deg);
        }
        
        .agent-id-input {
            width: 100%;
            padding: 14px 18px;
            border: 2px solid var(--border-color);
            border-radius: 12px;
            font-size: 16px;
            font-weight: 500;
            transition: all 0.2s;
            margin-bottom: 20px;
        }
        
        .agent-id-input:focus {
            outline: none;
            border-color: var(--primary-navy);
            box-shadow: 0 0 0 3px rgba(0, 56, 101, 0.1);
        }
        
        .search-agent-btn {
            background: linear-gradient(135deg, var(--primary-navy) 0%, var(--primary-navy-dark) 100%);
            color: white;
            padding: 12px 28px;
            border: none;
            border-radius: 12px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: var(--shadow-md);
            width: 100%;
            margin-bottom: 24px;
        }
        
        .search-agent-btn:hover {
            transform: translateY(-2px);
            box-shadow: var(--shadow-lg);
        }
        
        .agent-result {
            flex: 1;
            overflow-y: auto;
            padding-right: 12px;
            margin-bottom: 20px;
        }
        
        .agent-result::-webkit-scrollbar {
            width: 8px;
        }
        
        .agent-result::-webkit-scrollbar-track {
            background: var(--background);
            border-radius: 4px;
        }
        
        .agent-result::-webkit-scrollbar-thumb {
            background: var(--border-color);
            border-radius: 4px;
        }
        
        .agent-result::-webkit-scrollbar-thumb:hover {
            background: var(--text-secondary);
        }
        
        .result-actions {
            display: flex;
            gap: 12px;
            justify-content: flex-end;
            padding-top: 20px;
            border-top: 1px solid var(--border-color);
        }
        
        /* Notes Page Styles */
        .notes-container {
            width: 100%;
            max-width: 1200px;
            margin: 0 auto;
            padding: 60px 24px 24px;
        }
        
        .notes-title {
            font-size: 32px;
            font-weight: 700;
            color: var(--text-primary);
            margin-bottom: 12px;
            text-align: center;
        }
        
        .notes-subtitle {
            font-size: 18px;
            color: var(--text-secondary);
            text-align: center;
            margin-bottom: 48px;
        }
        
        .notes-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
            gap: 24px;
            margin-bottom: 32px;
        }
        
        .notes-section {
            background: var(--white);
            border-radius: 16px;
            padding: 28px;
            box-shadow: var(--shadow-sm);
            border: 1px solid var(--border-color);
            transition: all 0.3s ease;
        }
        
        .notes-section:hover {
            box-shadow: var(--shadow-md);
            border-color: var(--primary-navy);
        }
        
        .section-header {
            display: flex;
            align-items: center;
            gap: 12px;
            margin-bottom: 20px;
        }
        
        .section-icon {
            width: 40px;
            height: 40px;
            background: linear-gradient(135deg, rgba(0, 56, 101, 0.1) 0%, rgba(0, 56, 101, 0.05) 100%);
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-shrink: 0;
        }
        
        .section-icon svg {
            width: 24px;
            height: 24px;
            stroke: var(--primary-navy);
        }
        
        .section-title {
            font-size: 18px;
            font-weight: 600;
            color: var(--text-primary);
        }
        
        .notes-textarea {
            width: 100%;
            min-height: 120px;
            padding: 12px;
            border: 1px solid var(--border-color);
            border-radius: 10px;
            font-size: 14px;
            font-family: inherit;
            resize: vertical;
            transition: all 0.2s;
            background: var(--background);
        }
        
        .notes-textarea:focus {
            outline: none;
            border-color: var(--primary-navy);
            background: var(--white);
            box-shadow: 0 0 0 3px rgba(0, 56, 101, 0.1);
        }
        
        .save-button {
            background: linear-gradient(135deg, var(--primary-navy) 0%, var(--primary-navy-dark) 100%);
            color: white;
            padding: 14px 32px;
            border: none;
            border-radius: 12px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: var(--shadow-md);
            display: block;
            margin: 0 auto;
        }
        
        .save-button:hover {
            transform: translateY(-2px);
            box-shadow: var(--shadow-lg);
        }
        
        /* User Profile Modal Styles */
        .profile-modal {
            max-width: 700px;
        }
        
        .profile-content {
            flex: 1;
            overflow-y: auto;
        }
        
        .profile-header {
            display: flex;
            align-items: center;
            gap: 24px;
            padding: 24px;
            background: linear-gradient(135deg, rgba(0, 56, 101, 0.05) 0%, rgba(0, 56, 101, 0.02) 100%);
            border-radius: 16px;
            margin-bottom: 32px;
        }
        
        .profile-avatar {
            width: 80px;
            height: 80px;
            background: linear-gradient(135deg, var(--primary-navy) 0%, var(--primary-navy-light) 100%);
            color: white;
            border-radius: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 32px;
            font-weight: 600;
        }
        
        .profile-info h3 {
            font-size: 24px;
            font-weight: 600;
            color: var(--text-primary);
            margin-bottom: 4px;
        }
        
        .profile-info p {
            font-size: 16px;
            color: var(--text-secondary);
            margin-bottom: 2px;
        }
        
        .profile-email {
            font-size: 14px;
            color: var(--primary-navy);
        }
        
        .profile-section {
            margin-bottom: 32px;
            padding-bottom: 32px;
            border-bottom: 1px solid var(--border-color);
        }
        
        .profile-section:last-child {
            border-bottom: none;
        }
        
        .profile-section h4 {
            font-size: 18px;
            font-weight: 600;
            color: var(--text-primary);
            margin-bottom: 20px;
        }
        
        .info-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 20px;
        }
        
        .info-item label {
            display: block;
            font-size: 12px;
            font-weight: 600;
            color: var(--text-secondary);
            text-transform: uppercase;
            letter-spacing: 0.05em;
            margin-bottom: 4px;
        }
        
        .info-item span {
            font-size: 16px;
            color: var(--text-primary);
        }
        
        .preference-item {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 20px;
        }
        
        .preference-item label {
            font-size: 16px;
            font-weight: 500;
            color: var(--text-primary);
        }
        
        .theme-toggle {
            display: flex;
            gap: 8px;
            background: var(--background);
            padding: 4px;
            border-radius: 10px;
        }
        
        .theme-toggle.compact {
            width: auto;
            gap: 8px;
        }
        
        .theme-option {
            padding: 8px 16px;
            border: none;
            background: transparent;
            border-radius: 8px;
            cursor: pointer;
            font-size: 14px;
            font-weight: 500;
            color: var(--text-secondary);
            transition: all 0.2s;
            display: flex;
            align-items: center;
            gap: 6px;
        }
        
        .theme-option:hover {
            color: var(--text-primary);
        }
        
        .theme-option.active {
            background: var(--white);
            color: var(--primary-navy);
            box-shadow: var(--shadow-sm);
        }
        
        .preference-select {
            padding: 8px 16px;
            border: 1px solid var(--border-color);
            border-radius: 8px;
            background: var(--white);
            font-size: 14px;
            color: var(--text-primary);
            cursor: pointer;
        }
        
        .profile-actions {
            display: flex;
            gap: 12px;
            justify-content: flex-end;
            margin-top: 32px;
            padding-top: 32px;
            border-top: 1px solid var(--border-color);
        }
        
        /* Templates Modal Styles */
        .templates-modal {
            max-width: 900px;
        }
        
        .templates-content {
            flex: 1;
            overflow-y: auto;
        }
        
        .template-categories {
            display: flex;
            gap: 12px;
            margin-bottom: 32px;
            flex-wrap: wrap;
        }
        
        .category-btn {
            padding: 10px 20px;
            background: var(--white);
            border: 1px solid var(--border-color);
            border-radius: 20px;
            cursor: pointer;
            font-size: 14px;
            font-weight: 500;
            color: var(--text-secondary);
            transition: all 0.2s;
        }
        
        .category-btn:hover {
            border-color: var(--primary-navy);
            color: var(--primary-navy);
        }
        
        .category-btn.active {
            background: var(--primary-navy);
            color: white;
            border-color: var(--primary-navy);
        }
        
        .templates-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 24px;
        }
        
        .template-card {
            background: var(--white);
            border: 2px solid var(--border-color);
            border-radius: 16px;
            padding: 24px;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .template-card:hover {
            border-color: var(--primary-navy);
            transform: translateY(-4px);
            box-shadow: var(--shadow-lg);
        }
        
        .template-icon {
            width: 56px;
            height: 56px;
            background: linear-gradient(135deg, rgba(0, 56, 101, 0.1) 0%, rgba(0, 56, 101, 0.05) 100%);
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 16px;
        }
        
        .template-icon svg {
            stroke: var(--primary-navy);
        }
        
        .template-card h3 {
            font-size: 18px;
            font-weight: 600;
            color: var(--text-primary);
            margin-bottom: 8px;
        }
        
        .template-card p {
            font-size: 14px;
            color: var(--text-secondary);
            line-height: 1.6;
            margin-bottom: 16px;
        }
        
        .template-tags {
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
        }
        
        .tag {
            padding: 4px 12px;
            background: var(--background);
            border-radius: 12px;
            font-size: 12px;
            color: var(--text-secondary);
        }
        
        /* Recent Agents Modal Styles */
        .recent-modal {
            max-width: 700px;
        }
        
        .recent-content {
            flex: 1;
            overflow-y: auto;
        }
        
        .recent-agent {
            display: flex;
            align-items: center;
            gap: 16px;
            padding: 20px;
            background: var(--white);
            border: 1px solid var(--border-color);
            border-radius: 12px;
            margin-bottom: 16px;
            cursor: pointer;
            transition: all 0.2s;
        }
        
        .recent-agent:hover {
            border-color: var(--primary-navy);
            box-shadow: var(--shadow-sm);
        }
        
        .recent-agent-icon {
            width: 48px;
            height: 48px;
            background: linear-gradient(135deg, rgba(0, 56, 101, 0.1) 0%, rgba(0, 56, 101, 0.05) 100%);
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-shrink: 0;
        }
        
        .recent-agent-icon svg {
            stroke: var(--primary-navy);
        }
        
        .recent-agent-info {
            flex: 1;
        }
        
        .recent-agent-info h4 {
            font-size: 16px;
            font-weight: 600;
            color: var(--text-primary);
            margin-bottom: 4px;
        }
        
        .recent-agent-info p {
            font-size: 14px;
            color: var(--text-secondary);
            margin-bottom: 8px;
        }
        
        .agent-stats {
            display: flex;
            gap: 16px;
            font-size: 13px;
            color: var(--text-secondary);
        }
        
        .agent-stats strong {
            color: var(--text-primary);
        }
        
        .agent-action-btn {
            width: 36px;
            height: 36px;
            background: var(--background);
            border: 1px solid var(--border-color);
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.2s;
        }
        
        .agent-action-btn:hover {
            background: var(--primary-navy);
            border-color: var(--primary-navy);
        }
        
        .agent-action-btn:hover svg {
            stroke: white;
        }
        
        /* My Agents Modal Styles */
        .agents-modal {
            max-width: 1000px;
        }
        
        .agents-dashboard {
            flex: 1;
            overflow-y: auto;
        }
        
        .dashboard-stats {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 20px;
            margin-bottom: 32px;
        }
        
        .stat-card {
            background: linear-gradient(135deg, rgba(0, 56, 101, 0.05) 0%, rgba(0, 56, 101, 0.02) 100%);
            border: 1px solid var(--border-color);
            border-radius: 12px;
            padding: 24px;
            text-align: center;
        }
        
        .stat-card h3 {
            font-size: 36px;
            font-weight: 700;
            color: var(--primary-navy);
            margin-bottom: 4px;
        }
        
        .stat-card p {
            font-size: 14px;
            color: var(--text-secondary);
        }
        
        .agents-table {
            background: var(--white);
            border-radius: 12px;
            overflow: hidden;
            border: 1px solid var(--border-color);
        }
        
        .agents-table table {
            width: 100%;
            border-collapse: collapse;
        }
        
        .agents-table th {
            background: var(--background);
            padding: 16px;
            text-align: left;
            font-size: 12px;
            font-weight: 600;
            color: var(--text-secondary);
            text-transform: uppercase;
            letter-spacing: 0.05em;
        }
        
        .agents-table td {
            padding: 16px;
            border-top: 1px solid var(--border-color);
            font-size: 14px;
        }
        
        .type-badge {
            padding: 4px 12px;
            border-radius: 12px;
            font-size: 12px;
            font-weight: 500;
        }
        
        .type-badge.conversational {
            background: rgba(0, 166, 160, 0.1);
            color: var(--accent-teal);
        }
        
        .status-badge {
            padding: 4px 12px;
            border-radius: 12px;
            font-size: 12px;
            font-weight: 500;
        }
        
        .status-badge.active {
            background: rgba(16, 185, 129, 0.1);
            color: #10b981;
        }
        
        .table-actions {
            display: flex;
            gap: 8px;
        }
        
        .table-actions button {
            width: 32px;
            height: 32px;
            background: transparent;
            border: 1px solid var(--border-color);
            border-radius: 6px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.2s;
        }
        
        .table-actions button:hover {
            background: var(--background);
            border-color: var(--primary-navy);
        }
        
        .table-actions button:hover svg {
            stroke: var(--primary-navy);
        }
        
        /* Support Modal Styles */
        .support-modal {
            max-width: 700px;
        }
        
        .support-content {
            flex: 1;
            overflow-y: auto;
        }
        
        .support-search {
            margin-bottom: 32px;
        }
        
        .support-search-input {
            width: 100%;
            padding: 16px 20px;
            border: 2px solid var(--border-color);
            border-radius: 12px;
            font-size: 16px;
            transition: all 0.2s;
        }
        
        .support-search-input:focus {
            outline: none;
            border-color: var(--primary-navy);
            box-shadow: 0 0 0 3px rgba(0, 56, 101, 0.1);
        }
        
        .support-categories {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-bottom: 32px;
        }
        
        .support-card {
            background: var(--background);
            border: 1px solid var(--border-color);
            border-radius: 12px;
            padding: 24px;
            text-align: center;
            cursor: pointer;
            transition: all 0.2s;
        }
        
        .support-card:hover {
            border-color: var(--primary-navy);
            transform: translateY(-2px);
            box-shadow: var(--shadow-sm);
        }
        
        .support-card svg {
            stroke: var(--primary-navy);
            margin-bottom: 12px;
        }
        
        .support-card h3 {
            font-size: 16px;
            font-weight: 600;
            color: var(--text-primary);
            margin-bottom: 8px;
        }
        
        .support-card p {
            font-size: 14px;
            color: var(--text-secondary);
        }
        
        .support-footer {
            text-align: center;
            padding: 20px;
            background: var(--background);
            border-radius: 12px;
        }
        
        .support-footer p {
            font-size: 14px;
            color: var(--text-secondary);
        }
        
        .support-footer strong {
            color: var(--primary-navy);
        }
        
        /* Dark mode styles */
        body.dark {
            --text-primary: #f3f4f6;
            --text-secondary: #9ca3af;
            --border-color: #374151;
            --background: #111827;
            --white: #1f2937;
            --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.3);
            --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.4);
            --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.5);
            --shadow-xl: 0 20px 25px -5px rgba(0, 0, 0, 0.6);
        }
        
        body.dark .modal {
            background: #1f2937;
        }
        
        body.dark .sun-icon {
            color: #fbbf24;
        }
        
        /* Upload feedback */
        .upload-feedback {
            position: fixed;
            bottom: 24px;
            right: 24px;
            background: var(--accent-teal);
            color: white;
            padding: 14px 28px;
            border-radius: 12px;
            box-shadow: var(--shadow-lg);
            opacity: 0;
            transform: translateY(20px);
            transition: all 0.3s var(--transition-smooth);
            font-weight: 500;
            z-index: 3000;
        }
        
        .upload-feedback.show {
            opacity: 1;
            transform: translateY(0);
        }
        
        /* Drag and Drop */
        .input-container.drag-over {
            border-color: var(--primary-navy);
            background: rgba(0, 56, 101, 0.02);
        }
        
        .drag-overlay {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 56, 101, 0.1);
            border: 2px dashed var(--primary-navy);
            border-radius: 20px;
            display: none;
            align-items: center;
            justify-content: center;
            z-index: 10;
        }
        
        .input-container.drag-over .drag-overlay {
            display: flex;
        }
        
        .drag-text {
            font-size: 16px;
            font-weight: 600;
            color: var(--primary-navy);
        }
        
        /* Keyboard shortcuts hint */
        .shortcuts-hint {
            position: absolute;
            bottom: -20px;
            right: 0;
            font-size: 11px;
            color: var(--text-secondary);
            opacity: 0.6;
        }
    </style>
</head>
<body>
    <!-- Sidebar -->
    <div class="sidebar">
        <div class="sidebar-icon active" onclick="navigateToHome()" title="Home">
            <svg viewBox="0 0 24 24">
                <path d="M3 9l9-7 9 7v11a2 2 0 01-2 2H5a2 2 0 01-2-2z"/>
                <polyline points="9 22 9 12 15 12 15 22"/>
            </svg>
        </div>
        
        <!-- New Session Button -->
        <div class="new-session-btn" onclick="createNewSession()" title="New Session">
            +
        </div>
        
        <!-- Chat History Button -->
        <div class="sidebar-icon" onclick="toggleChatHistory()" title="Chat History">
            <svg viewBox="0 0 24 24">
                <path d="M21 15a2 2 0 01-2 2H7l-4 4V5a2 2 0 012-2h14a2 2 0 012 2z"/>
                <path d="M8 10h8m-8 4h4"/>
            </svg>
            <div class="chat-history-dropdown" id="chatHistoryDropdown">
                <div class="chat-history-header">Chat History</div>
                <div class="chat-item" onclick="resumeChat('chat-001')">
                    <div class="chat-title">Customer Service Bot Setup</div>
                    <div class="chat-preview">I need an agent that can handle customer inquiries...</div>
                    <div class="chat-date">2 hours ago</div>
                </div>
                <div class="chat-item" onclick="resumeChat('chat-002')">
                    <div class="chat-title">Data Analyst Agent</div>
                    <div class="chat-preview">Create an agent for analyzing financial reports...</div>
                    <div class="chat-date">Yesterday</div>
                </div>
                <div class="chat-item" onclick="resumeChat('chat-003')">
                    <div class="chat-title">Process Automation Setup</div>
                    <div class="chat-preview">Build an agent to automate invoice processing...</div>
                    <div class="chat-date">3 days ago</div>
                </div>
                <div class="chat-item" onclick="resumeChat('chat-004')">
                    <div class="chat-title">Compliance Checker Agent</div>
                    <div class="chat-preview">Need an agent for regulatory compliance checks...</div>
                    <div class="chat-date">Last week</div>
                </div>
            </div>
        </div>
        
        <div class="sidebar-icon" onclick="openMyAgents()" title="My Agents">
            <svg viewBox="0 0 24 24">
                <path d="M16 20h3a2 2 0 002-2V6a2 2 0 00-2-2h-3"/>
                <path d="M8 20H5a2 2 0 01-2-2V6a2 2 0 012-2h3"/>
                <path d="M12 2v20"/>
                <path d="M9 12h6"/>
            </svg>
        </div>
        <div class="sidebar-icon" onclick="openSupport()" title="Help & Support">
            <svg viewBox="0 0 24 24">
                <circle cx="12" cy="12" r="10"/>
                <path d="M9.09 9a3 3 0 015.83 1c0 2-3 3-3 3"/>
                <line x1="12" y1="17" x2="12.01" y2="17"/>
            </svg>
        </div>
        <div class="sidebar-icon" style="margin-top: auto;" onclick="openUserProfile()" title="User Profile">
            <span style="font-size: 18px; font-weight: 600; color: var(--text-secondary);">K</span>
        </div>
    </div>
    
    <!-- Main Content -->
    <div class="main-content initial" id="mainContent">
        <!-- Home Page -->
        <div class="page active" id="homePage">
            <div class="greeting">
                <span class="bny-logo">BNY</span>
                <span style="font-weight: 400;">Welcome back, <span id="userName">Kondareddy</span>! 👋</span>
            </div>
            
            <!-- Mode Selector Tabs -->
            <div class="mode-selector">
                <button class="mode-tab active" onclick="selectMode(this, 'create')">Create</button>
                <button class="mode-tab" onclick="selectMode(this, 'maintenance')">Maintenance</button>
                <button class="mode-tab" onclick="selectMode(this, 'general')">General Questions</button>
            </div>
            
            <div class="input-container" id="mainInputContainer">
                <div class="drag-overlay">
                    <span class="drag-text">Drop files here to attach</span>
                </div>
                <div class="input-wrapper">
                    <textarea class="input-field" id="mainInput" placeholder="Describe the agent you want to create..." rows="1"></textarea>
                    <span class="char-count">0 / 4000</span>
                </div>
                
                <div class="actions-attachments-wrapper">
                    <div class="input-actions">
                        <div class="left-actions">
                            <label class="action-btn" for="inline-file-upload" title="Attach files">
                                <svg viewBox="0 0 24 24" fill="none">
                                    <path d="M21.44 11.05l-9.19 9.19a6 6 0 01-8.49-8.49l9.19-9.19a4 4 0 015.66 5.66l-9.2 9.19a2 2 0 01-2.83-2.83l8.49-8.48" stroke-linecap="round" stroke-linejoin="round"/>
                                </svg>
                                <input type="file" id="inline-file-upload" style="display: none;" multiple accept=".pdf,.doc,.docx,.txt,.csv,.xlsx,.jpg,.jpeg,.png">
                            </label>
                            <div class="action-btn" onclick="toggleOptionsDropdown(this)" title="More options">
                                <svg viewBox="0 0 24 24" fill="none">
                                    <path d="M4 6h16M4 12h16M4 18h16" stroke-linecap="round" stroke-linejoin="round"/>
                                </svg>
                                <div class="options-dropdown" id="optionsDropdown">
                                    <div class="option-item">
                                        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                                            <path d="M9 11l3 3L22 4"/>
                                            <path d="M21 12v7a2 2 0 01-2 2H5a2 2 0 01-2-2V5a2 2 0 012-2h11"/>
                                        </svg>
                                        Extended Thinking
                                    </div>
                                    <div class="option-item">
                                        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                                            <rect x="2" y="5" width="20" height="14" rx="2"/>
                                            <line x1="8" y1="12" x2="16" y2="12"/>
                                            <path d="M12 9v6"/>
                                        </svg>
                                        Payments Knowledge
                                    </div>
                                </div>
                            </div>
                            <button class="web-search-btn" onclick="toggleWebSearch(this)">
                                <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
                                    <circle cx="11" cy="11" r="8" stroke="currentColor"/>
                                    <path d="m21 21-4.35-4.35" stroke="currentColor" stroke-linecap="round"/>
                                </svg>
                                Web Search
                            </button>
                        </div>
                        <div style="display: flex; align-items: center; gap: 12px;">
                            <div class="agent-dropdown-container">
                                <button class="agent-selector" onclick="toggleAgentDropdown(this)">
                                    <span class="agent-name">Worker Agent</span>
                                    <svg class="dropdown-arrow" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                                        <path d="M6 9l6 6 6-6" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                                    </svg>
                                </button>
                                <div class="agent-dropdown" id="agentDropdown">
                                    <div class="agent-option selected" onclick="selectAgent(this, 'Worker Agent', 'worker')">
                                        <div class="agent-option-content">
                                            <span class="agent-option-name">Worker Agent</span>
                                            <span class="agent-option-desc">Default system agent</span>
                                        </div>
                                    </div>
                                    <div class="agent-option" onclick="selectAgent(this, 'Customer Service Bot', 'cs-001')">
                                        <div class="agent-option-content">
                                            <span class="agent-option-name">Customer Service Bot</span>
                                            <span class="agent-option-desc">Handles customer inquiries</span>
                                        </div>
                                    </div>
                                    <div class="agent-option" onclick="selectAgent(this, 'Data Analyst Agent', 'da-002')">
                                        <div class="agent-option-content">
                                            <span class="agent-option-name">Data Analyst Agent</span>
                                            <span class="agent-option-desc">Analyzes financial data</span>
                                        </div>
                                    </div>
                                    <div class="agent-option" onclick="selectAgent(this, 'Process Automation', 'pa-003')">
                                        <div class="agent-option-content">
                                            <span class="agent-option-name">Process Automation</span>
                                            <span class="agent-option-desc">Automates workflows</span>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <button class="send-btn" onclick="sendMessage()" title="Send message (Enter)">↑</button>
                        </div>
                    </div>
                    
                    <!-- File Attachments Container -->
                    <div class="attachments-container" id="attachmentsContainer"></div>
                </div>
                
                <span class="shortcuts-hint">Press Enter to send • Shift+Enter for new line</span>
            </div>
            
            <div class="quick-actions">
                <button class="quick-action" onclick="openAgentTemplates()">
                    <svg viewBox="0 0 24 24" fill="none">
                        <path d="M4 5h16a1 1 0 011 1v12a1 1 0 01-1 1H4a1 1 0 01-1-1V6a1 1 0 011-1z"/>
                        <path d="M7 10h10m-10 4h6"/>
                    </svg>
                    Agent Templates
                </button>
                <button class="quick-action" onclick="viewRecentAgents()">
                    <svg viewBox="0 0 24 24" fill="none">
                        <circle cx="12" cy="12" r="10"/>
                        <polyline points="12 6 12 12 16 14"/>
                    </svg>
                    Recent Agents
                </button>
                <button class="quick-action" onclick="openBNYNews()">
                    <svg viewBox="0 0 24 24" fill="none">
                        <path d="M2 3h6a4 4 0 014 4v14a3 3 0 00-3-3H2z"/>
                        <path d="M22 3h-6a4 4 0 00-4 4v14a3 3 0 013-3h7z"/>
                    </svg>
                    BNY AI News
                </button>
            </div>
            
            <div class="projects-section">
                <h2 class="section-title">Quick Start Options</h2>
                <div class="project-cards">
                    <div class="project-card" onclick="openAgentSearch()">
                        <div class="project-icon">
                            <svg viewBox="0 0 24 24" fill="none">
                                <circle cx="11" cy="11" r="8"/>
                                <path d="m21 21-4.35-4.35"/>
                                <path d="M8 11h6m-3-3v6"/>
                            </svg>
                        </div>
                        <h3 class="project-title">Understanding an Agent</h3>
                        <p class="project-description">Get to know about an agent that you have access to by providing the agent ID</p>
                    </div>
                    
                    <div class="project-card" onclick="openAgentNotes()">
                        <div class="project-icon">
                            <svg viewBox="0 0 24 24" fill="none">
                                <path d="M14 2H6a2 2 0 00-2 2v16a2 2 0 002 2h12a2 2 0 002-2V8z"/>
                                <polyline points="14 2 14 8 20 8"/>
                                <line x1="16" y1="13" x2="8" y2="13"/>
                                <line x1="16" y1="17" x2="8" y2="17"/>
                                <polyline points="10 9 9 9 8 9"/>
                            </svg>
                        </div>
                        <h3 class="project-title">Agent Documentation</h3>
                        <p class="project-description">Store important details like architecture, tech stack, credentials, and resources</p>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Agent Building Flow Page -->
        <div class="page" id="agentFlowPage">
            <button class="back-to-home" onclick="navigateToHome()" title="Back to home">
                <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                    <path d="M19 12H5m0 0l7 7m-7-7l7-7" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
                Back
            </button>
            <div class="flow-container" id="flowContainer">
                <!-- Flow steps will appear here -->
            </div>
            
            <div class="bottom-search-container">
                <div class="bottom-search-wrapper">
                    <div class="input-container" id="flowInputContainer">
                        <div class="drag-overlay">
                            <span class="drag-text">Drop files here to attach</span>
                        </div>
                        <div class="input-wrapper">
                            <textarea class="input-field" id="flowInput" placeholder="Describe what you want your agent to do..." rows="1"></textarea>
                            <span class="char-count">0 / 4000</span>
                        </div>
                        
                        <div class="actions-attachments-wrapper">
                            <div class="input-actions">
                                <div class="left-actions">
                                    <label class="action-btn" for="flow-file-upload" title="Attach files">
                                        <svg viewBox="0 0 24 24" fill="none">
                                            <path d="M21.44 11.05l-9.19 9.19a6 6 0 01-8.49-8.49l9.19-9.19a4 4 0 015.66 5.66l-9.2 9.19a2 2 0 01-2.83-2.83l8.49-8.48" stroke-linecap="round" stroke-linejoin="round"/>
                                        </svg>
                                        <input type="file" id="flow-file-upload" style="display: none;" multiple accept=".pdf,.doc,.docx,.txt,.csv,.xlsx,.jpg,.jpeg,.png">
                                    </label>
                                    <div class="action-btn" onclick="toggleOptionsDropdown(this)" title="More options">
                                        <svg viewBox="0 0 24 24" fill="none">
                                            <path d="M4 6h16M4 12h16M4 18h16" stroke-linecap="round" stroke-linejoin="round"/>
                                        </svg>
                                        <div class="options-dropdown">
                                            <div class="option-item">
                                                <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                                                    <path d="M9 11l3 3L22 4"/>
                                                    <path d="M21 12v7a2 2 0 01-2 2H5a2 2 0 01-2-2V5a2 2 0 012-2h11"/>
                                                </svg>
                                                Extended Thinking
                                            </div>
                                            <div class="option-item">
                                                <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                                                    <rect x="2" y="5" width="20" height="14" rx="2"/>
                                                    <line x1="8" y1="12" x2="16" y2="12"/>
                                                    <path d="M12 9v6"/>
                                                </svg>
                                                Payments Knowledge
                                            </div>
                                        </div>
                                    </div>
                                    <button class="web-search-btn" onclick="toggleWebSearch(this)">
                                        <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
                                            <circle cx="11" cy="11" r="8" stroke="currentColor"/>
                                            <path d="m21 21-4.35-4.35" stroke="currentColor" stroke-linecap="round"/>
                                        </svg>
                                        Web Search
                                    </button>
                                </div>
                                <div style="display: flex; align-items: center; gap: 12px;">
                                    <div class="agent-dropdown-container">
                                        <button class="agent-selector" onclick="toggleAgentDropdown(this)">
                                            <span class="agent-name">Worker Agent</span>
                                            <svg class="dropdown-arrow" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                                                <path d="M6 9l6 6 6-6" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                                            </svg>
                                        </button>
                                        <div class="agent-dropdown">
                                            <div class="agent-option selected" onclick="selectAgent(this, 'Worker Agent', 'worker')">
                                                <div class="agent-option-content">
                                                    <span class="agent-option-name">Worker Agent</span>
                                                    <span class="agent-option-desc">Default system agent</span>
                                                </div>
                                            </div>
                                            <div class="agent-option" onclick="selectAgent(this, 'Customer Service Bot', 'cs-001')">
                                                <div class="agent-option-content">
                                                    <span class="agent-option-name">Customer Service Bot</span>
                                                    <span class="agent-option-desc">Handles customer inquiries</span>
                                                </div>
                                            </div>
                                            <div class="agent-option" onclick="selectAgent(this, 'Data Analyst Agent', 'da-002')">
                                                <div class="agent-option-content">
                                                    <span class="agent-option-name">Data Analyst Agent</span>
                                                    <span class="agent-option-desc">Analyzes financial data</span>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                    <button class="send-btn" onclick="sendMessage()" title="Send message (Enter)">↑</button>
                                </div>
                            </div>
                            
                            <!-- File Attachments Container -->
                            <div class="attachments-container" id="flowAttachmentsContainer"></div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    
    <!-- Agent Search Modal -->
    <div class="modal-overlay" id="agentSearchModal">
        <div class="modal">
            <div class="modal-header">
                <h2 class="modal-title">Understanding an Agent</h2>
                <button class="close-modal" onclick="closeAgentSearch()">
                    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                        <path d="M6 18L18 6M6 6l12 12" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    </svg>
                </button>
            </div>
            
            <input type="text" class="agent-id-input" placeholder="Enter Agent ID (e.g., AGT-2024-001)" id="agentIdInput">
            <button class="search-agent-btn" onclick="searchAgent()">Search Agent</button>
            
            <div class="agent-result" id="agentResult">
                <!-- Results will be populated here -->
            </div>
            
            <div class="result-actions" id="resultActions" style="display: none;">
                <button class="action-button secondary" onclick="copyResults()">
                    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                        <rect x="9" y="9" width="13" height="13" rx="2" ry="2"/>
                        <path d="M5 15H4a2 2 0 01-2-2V4a2 2 0 012-2h9a2 2 0 012 2v1"/>
                    </svg>
                    Copy
                </button>
                <button class="action-button primary" onclick="downloadPDF()">
                    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                        <path d="M21 15v4a2 2 0 01-2 2H5a2 2 0 01-2-2v-4"/>
                        <polyline points="7 10 12 15 17 10"/>
                        <line x1="12" y1="15" x2="12" y2="3"/>
                    </svg>
                    Download PDF
                </button>
            </div>
        </div>
    </div>
    
    <!-- Agent Notes Page -->
    <div class="page" id="agentNotesPage">
        <button class="back-to-home" onclick="navigateToHome()" title="Back to home">
            <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                <path d="M19 12H5m0 0l7 7m-7-7l7-7" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
            Back
        </button>
        
        <div class="notes-container">
            <h1 class="notes-title">Agent Documentation</h1>
            <p class="notes-subtitle">Document important details about your agent for future reference</p>
            
            <div class="notes-grid">
                <div class="notes-section">
                    <div class="section-header">
                        <div class="section-icon">
                            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
                                <path d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6"/>
                            </svg>
                        </div>
                        <h3 class="section-title">Architecture</h3>
                    </div>
                    <textarea class="notes-textarea" placeholder="Describe your agent's architecture, design patterns, and system components..."></textarea>
                </div>
                
                <div class="notes-section">
                    <div class="section-header">
                        <div class="section-icon">
                            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
                                <path d="M10 20l4-16m4 4l4 4-4 4M6 16l-4-4 4-4"/>
                            </svg>
                        </div>
                        <h3 class="section-title">Tech Stack & Logic</h3>
                    </div>
                    <textarea class="notes-textarea" placeholder="List the technologies, frameworks, and logic patterns used in your agent..."></textarea>
                </div>
                
                <div class="notes-section">
                    <div class="section-header">
                        <div class="section-icon">
                            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
                                <path d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2z"/>
                                <path d="M12 11v4"/>
                                <circle cx="12" cy="11" r="1"/>
                            </svg>
                        </div>
                        <h3 class="section-title">Credentials</h3>
                    </div>
                    <textarea class="notes-textarea" placeholder="Document API keys, authentication methods, and credential management (keep sensitive data secure)..."></textarea>
                </div>
                
                <div class="notes-section">
                    <div class="section-header">
                        <div class="section-icon">
                            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
                                <path d="M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5"/>
                            </svg>
                        </div>
                        <h3 class="section-title">Resources & Sources</h3>
                    </div>
                    <textarea class="notes-textarea" placeholder="List all data sources, APIs, documentation, and resources provided to your agent..."></textarea>
                </div>
            </div>
            
            <button class="save-button" onclick="saveAgentNotes()">Save Notes</button>
        </div>
    </div>
    
    <!-- User Profile Modal -->
    <div class="modal-overlay" id="userProfileModal">
        <div class="modal profile-modal">
            <div class="modal-header">
                <h2 class="modal-title">User Profile</h2>
                <button class="close-modal" onclick="closeUserProfile()">
                    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                        <path d="M6 18L18 6M6 6l12 12" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    </svg>
                </button>
            </div>
            
            <div class="profile-content">
                <div class="profile-header">
                    <div class="profile-avatar">K</div>
                    <div class="profile-info">
                        <h3>Kondareddy</h3>
                        <p>Senior Developer - AI Solutions</p>
                        <p class="profile-email">kondareddy@bny.com</p>
                    </div>
                </div>
                
                <div class="profile-section">
                    <h4>Account Information</h4>
                    <div class="info-grid">
                        <div class="info-item">
                            <label>Employee ID</label>
                            <span>BNY-2024-1234</span>
                        </div>
                        <div class="info-item">
                            <label>Department</label>
                            <span>Technology & Innovation</span>
                        </div>
                        <div class="info-item">
                            <label>Location</label>
                            <span>New York, NY</span>
                        </div>
                        <div class="info-item">
                            <label>Member Since</label>
                            <span>January 2024</span>
                        </div>
                    </div>
                </div>
                
                <div class="profile-section">
                    <h4>Preferences</h4>
                    <div class="preference-item">
                        <label>Theme</label>
                        <div class="theme-toggle compact">
                            <button class="theme-option active" onclick="setTheme('light')">Light</button>
                            <button class="theme-option" onclick="setTheme('dark')">Dark</button>
                        </div>
                    </div>
                    <div class="preference-item">
                        <label>Language</label>
                        <select class="preference-select">
                            <option>English</option>
                            <option>Spanish</option>
                            <option>French</option>
                        </select>
                    </div>
                    <div class="preference-item">
                        <label>Notification Settings</label>
                        <select class="preference-select">
                            <option>All Notifications</option>
                            <option>Important Only</option>
                            <option>None</option>
                        </select>
                    </div>
                </div>
                
                <div class="profile-actions">
                    <button class="action-button secondary">Change Password</button>
                    <button class="action-button primary">Save Changes</button>
                </div>
            </div>
        </div>
    </div>
    
    <!-- Agent Templates Modal -->
    <div class="modal-overlay" id="templatesModal">
        <div class="modal templates-modal">
            <div class="modal-header">
                <h2 class="modal-title">Agent Templates</h2>
                <button class="close-modal" onclick="closeTemplatesModal()">
                    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                        <path d="M6 18L18 6M6 6l12 12" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    </svg>
                </button>
            </div>
            
            <div class="templates-content">
                <div class="template-categories">
                    <button class="category-btn active">All Templates</button>
                    <button class="category-btn">Customer Service</button>
                    <button class="category-btn">Data Analysis</button>
                    <button class="category-btn">Process Automation</button>
                </div>
                
                <div class="templates-grid">
                    <div class="template-card" onclick="useTemplate('customer-support')">
                        <div class="template-icon">
                            <svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                                <path d="M21 15a2 2 0 01-2 2H7l-4 4V5a2 2 0 012-2h14a2 2 0 012 2z"/>
                            </svg>
                        </div>
                        <h3>Customer Support Agent</h3>
                        <p>Pre-configured agent for handling customer inquiries with sentiment analysis</p>
                        <div class="template-tags">
                            <span class="tag">NLP</span>
                            <span class="tag">CRM Integration</span>
                        </div>
                    </div>
                    
                    <div class="template-card" onclick="useTemplate('data-analyst')">
                        <div class="template-icon">
                            <svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                                <line x1="18" y1="20" x2="18" y2="10"/>
                                <line x1="12" y1="20" x2="12" y2="4"/>
                                <line x1="6" y1="20" x2="6" y2="14"/>
                            </svg>
                        </div>
                        <h3>Data Analyst Agent</h3>
                        <p>Analyzes data patterns and generates insights with visualization capabilities</p>
                        <div class="template-tags">
                            <span class="tag">Python</span>
                            <span class="tag">SQL</span>
                            <span class="tag">Charts</span>
                        </div>
                    </div>
                    
                    <div class="template-card" onclick="useTemplate('compliance-checker')">
                        <div class="template-icon">
                            <svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                                <path d="M9 11l3 3L22 4"/>
                                <path d="M21 12v7a2 2 0 01-2 2H5a2 2 0 01-2-2V5a2 2 0 012-2h11"/>
                            </svg>
                        </div>
                        <h3>Compliance Checker</h3>
                        <p>Automated compliance verification for financial regulations and policies</p>
                        <div class="template-tags">
                            <span class="tag">RegTech</span>
                            <span class="tag">Audit Trail</span>
                        </div>
                    </div>
                    
                    <div class="template-card" onclick="useTemplate('report-generator')">
                        <div class="template-icon">
                            <svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                                <path d="M14 2H6a2 2 0 00-2 2v16a2 2 0 002 2h12a2 2 0 002-2V8z"/>
                                <polyline points="14 2 14 8 20 8"/>
                            </svg>
                        </div>
                        <h3>Report Generator</h3>
                        <p>Automatically generates reports from multiple data sources with formatting</p>
                        <div class="template-tags">
                            <span class="tag">PDF Export</span>
                            <span class="tag">Scheduling</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    
    <!-- Recent Agents Modal -->
    <div class="modal-overlay" id="recentAgentsModal">
        <div class="modal recent-modal">
            <div class="modal-header">
                <h2 class="modal-title">Recent Agents</h2>
                <button class="close-modal" onclick="closeRecentAgents()">
                    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                        <path d="M6 18L18 6M6 6l12 12" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    </svg>
                </button>
            </div>
            
            <div class="recent-content">
                <div class="recent-agent" onclick="loadAgent('cs-001')">
                    <div class="recent-agent-icon">
                        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                            <circle cx="12" cy="12" r="10"/>
                            <path d="M8 14s1.5 2 4 2 4-2 4-2"/>
                        </svg>
                    </div>
                    <div class="recent-agent-info">
                        <h4>Customer Service Bot</h4>
                        <p>Last modified: 2 hours ago</p>
                        <div class="agent-stats">
                            <span><strong>95%</strong> Success Rate</span>
                            <span><strong>1.2k</strong> Interactions</span>
                        </div>
                    </div>
                    <button class="agent-action-btn">
                        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                            <path d="M12 5v14m7-7H5"/>
                        </svg>
                    </button>
                </div>
                
                <div class="recent-agent" onclick="loadAgent('da-002')">
                    <div class="recent-agent-icon">
                        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                            <line x1="18" y1="20" x2="18" y2="10"/>
                            <line x1="12" y1="20" x2="12" y2="4"/>
                            <line x1="6" y1="20" x2="6" y2="14"/>
                        </svg>
                    </div>
                    <div class="recent-agent-info">
                        <h4>Data Analyst Agent</h4>
                        <p>Last modified: Yesterday</p>
                        <div class="agent-stats">
                            <span><strong>847</strong> Reports Generated</span>
                            <span><strong>99.2%</strong> Accuracy</span>
                        </div>
                    </div>
                    <button class="agent-action-btn">
                        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                            <path d="M12 5v14m7-7H5"/>
                        </svg>
                    </button>
                </div>
                
                <div class="recent-agent" onclick="loadAgent('pa-003')">
                    <div class="recent-agent-icon">
                        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                            <path d="M13 2L3 14h9l-1 8 10-12h-9l1-8z"/>
                        </svg>
                    </div>
                    <div class="recent-agent-info">
                        <h4>Process Automation Agent</h4>
                        <p>Last modified: 3 days ago</p>
                        <div class="agent-stats">
                            <span><strong>15.3k</strong> Tasks Completed</span>
                            <span><strong>4h</strong> Avg. Saved/Day</span>
                        </div>
                    </div>
                    <button class="agent-action-btn">
                        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                            <path d="M12 5v14m7-7H5"/>
                        </svg>
                    </button>
                </div>
            </div>
        </div>
    </div>
    
    <!-- My Agents Modal -->
    <div class="modal-overlay" id="myAgentsModal">
        <div class="modal agents-modal">
            <div class="modal-header">
                <h2 class="modal-title">My Agents</h2>
                <button class="close-modal" onclick="closeMyAgents()">
                    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                        <path d="M6 18L18 6M6 6l12 12" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    </svg>
                </button>
            </div>
            
            <div class="agents-dashboard">
                <div class="dashboard-stats">
                    <div class="stat-card">
                        <h3>12</h3>
                        <p>Total Agents</p>
                    </div>
                    <div class="stat-card">
                        <h3>8</h3>
                        <p>Active</p>
                    </div>
                    <div class="stat-card">
                        <h3>45.2k</h3>
                        <p>Total Interactions</p>
                    </div>
                    <div class="stat-card">
                        <h3>98.5%</h3>
                        <p>Avg Success Rate</p>
                    </div>
                </div>
                
                <div class="agents-table">
                    <table>
                        <thead>
                            <tr>
                                <th>Agent Name</th>
                                <th>Type</th>
                                <th>Status</th>
                                <th>Created</th>
                                <th>Actions</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>Customer Service Bot</td>
                                <td><span class="type-badge conversational">Conversational</span></td>
                                <td><span class="status-badge active">Active</span></td>
                                <td>Jan 15, 2024</td>
                                <td>
                                    <div class="table-actions">
                                        <button title="Edit"><svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor"><path d="M11 4H4a2 2 0 00-2 2v14a2 2 0 002 2h14a2 2 0 002-2v-7"/><path d="M18.5 2.5a2.121 2.121 0 113 3L12 15l-4 1 1-4 9.5-9.5z"/></svg></button>
                                        <button title="View Details"><svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor"><path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/><circle cx="12" cy="12" r="3"/></svg></button>
                                        <button title="Delete"><svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor"><polyline points="3 6 5 6 21 6"/><path d="M19 6v14a2 2 0 01-2 2H7a2 2 0 01-2-2V6m3 0V4a2 2 0 012-2h4a2 2 0 012 2v2"/></svg></button>
                                    </div>
                                </td>
                            </tr>
                            <tr>
                                <td>Data Analyst Agent</td>
                                <td><span class="type-badge conversational">Analytics</span></td>
                                <td><span class="status-badge active">Active</span></td>
                                <td>Feb 02, 2024</td>
                                <td>
                                    <div class="table-actions">
                                        <button title="Edit"><svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor"><path d="M11 4H4a2 2 0 00-2 2v14a2 2 0 002 2h14a2 2 0 002-2v-7"/><path d="M18.5 2.5a2.121 2.121 0 113 3L12 15l-4 1 1-4 9.5-9.5z"/></svg></button>
                                        <button title="View Details"><svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor"><path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/><circle cx="12" cy="12" r="3"/></svg></button>
                                        <button title="Delete"><svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor"><polyline points="3 6 5 6 21 6"/><path d="M19 6v14a2 2 0 01-2 2H7a2 2 0 01-2-2V6m3 0V4a2 2 0 012-2h4a2 2 0 012 2v2"/></svg></button>
                                    </div>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
    
    <!-- Help & Support Modal -->
    <div class="modal-overlay" id="supportModal">
        <div class="modal support-modal">
            <div class="modal-header">
                <h2 class="modal-title">Help & Support</h2>
                <button class="close-modal" onclick="closeSupport()">
                    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                        <path d="M6 18L18 6M6 6l12 12" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    </svg>
                </button>
            </div>
            
            <div class="support-content">
                <div class="support-search">
                    <input type="text" placeholder="Search for help..." class="support-search-input">
                </div>
                
                <div class="support-categories">
                    <div class="support-card">
                        <svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                            <path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2z"/>
                        </svg>
                        <h3>Getting Started</h3>
                        <p>Learn the basics of creating your first agent</p>
                    </div>
                    
                    <div class="support-card">
                        <svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                            <circle cx="12" cy="12" r="10"/>
                            <path d="M9.09 9a3 3 0 015.83 1c0 2-3 3-3 3"/>
                            <line x1="12" y1="17" x2="12.01" y2="17"/>
                        </svg>
                        <h3>FAQs</h3>
                        <p>Find answers to common questions</p>
                    </div>
                    
                    <div class="support-card">
                        <svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                            <path d="M21 15a2 2 0 01-2 2H7l-4 4V5a2 2 0 012-2h14a2 2 0 012 2z"/>
                        </svg>
                        <h3>Contact Support</h3>
                        <p>Get help from our support team</p>
                    </div>
                </div>
                
                <div class="support-footer">
                    <p>Need immediate assistance? Call our support line: <strong>1-800-BNY-HELP</strong></p>
                </div>
            </div>
        </div>
    </div>
    
    <!-- Upload Feedback -->
    <div class="upload-feedback" id="uploadFeedback">
        Document uploaded successfully!
    </div>
    
    <script>
        let attachments = [];
        let flowAttachments = [];
        let currentStep = 0;
        let agentType = '';
        let currentChatId = null;
        let chatHistory = [];
        
        // Template sample texts
        const templateSamples = {
            'customer-support': 'I need a customer support agent that can handle inquiries from multiple channels (email, chat, phone). The agent should be able to identify customer sentiment, categorize issues automatically, escalate complex cases to human agents, and integrate with our existing CRM system (Salesforce). It should maintain conversation history and provide multilingual support for English, Spanish, and French.',
            'data-analyst': 'Build a data analyst agent that can connect to our SQL databases and Excel files to perform automated data analysis. It should be able to generate daily/weekly reports, identify trends and anomalies in financial data, create visualizations and dashboards, and send alerts when certain thresholds are met. The agent should support scheduled analysis runs and export results to PDF and Excel formats.',
            'compliance-checker': 'Create a compliance checking agent that monitors all financial transactions for regulatory compliance. It should validate transactions against current regulations (SOX, GDPR, AML), flag suspicious activities, generate audit trails, and produce compliance reports. The agent needs to stay updated with regulatory changes and integrate with our existing risk management systems.',
            'report-generator': 'I want an agent that automatically generates comprehensive business reports by pulling data from multiple sources including databases, APIs, and spreadsheets. It should format reports according to our brand guidelines, include charts and visualizations, schedule report generation (daily/weekly/monthly), and distribute reports via email to specified recipients with role-based access control.'
        };
        
        // Auto-resize textarea
        function autoResize(textarea) {
            textarea.style.height = 'auto';
            const maxHeight = 120; // 5 lines approximately
            textarea.style.height = Math.min(textarea.scrollHeight, maxHeight) + 'px';
        }
        
        // Setup textareas
        document.querySelectorAll('.input-field').forEach(textarea => {
            textarea.addEventListener('input', function() {
                autoResize(this);
                updateCharCount(this);
            });
            
            textarea.addEventListener('keydown', function(e) {
                if (e.key === 'Enter' && !e.shiftKey) {
                    e.preventDefault();
                    sendMessage();
                }
            });
        });
        
        // Character count
        function updateCharCount(textarea) {
            const charCount = textarea.parentElement.querySelector('.char-count');
            if (charCount) {
                charCount.textContent = `${textarea.value.length} / 4000`;
            }
        }
        
        // File size formatter
        function formatFileSize(bytes) {
            if (bytes === 0) return '0 Bytes';
            const k = 1024;
            const sizes = ['Bytes', 'KB', 'MB', 'GB'];
            const i = Math.floor(Math.log(bytes) / Math.log(k));
            return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
        }
        
        // Drag and drop
        const setupDragAndDrop = (container, context) => {
            if (!container) return;
            
            container.addEventListener('dragover', (e) => {
                e.preventDefault();
                container.classList.add('drag-over');
            });
            
            container.addEventListener('dragleave', (e) => {
                if (e.target === container) {
                    container.classList.remove('drag-over');
                }
            });
            
            container.addEventListener('drop', (e) => {
                e.preventDefault();
                container.classList.remove('drag-over');
                
                const files = Array.from(e.dataTransfer.files);
                handleFiles(files, context);
            });
        };
        
        setupDragAndDrop(document.getElementById('mainInputContainer'), 'main');
        setupDragAndDrop(document.getElementById('flowInputContainer'), 'flow');
        
        // File upload handlers
        document.getElementById('inline-file-upload').addEventListener('change', handleInlineFileUpload);
        document.getElementById('flow-file-upload').addEventListener('change', handleFlowFileUpload);
        
        function handleInlineFileUpload(e) {
            handleFiles(Array.from(e.target.files), 'main');
            e.target.value = '';
        }
        
        function handleFlowFileUpload(e) {
            handleFiles(Array.from(e.target.files), 'flow');
            e.target.value = '';
        }
        
        function handleFiles(files, context) {
            const container = context === 'main' ? 
                document.getElementById('attachmentsContainer') : 
                document.getElementById('flowAttachmentsContainer');
            
            files.forEach(file => {
                if (context === 'main') {
                    attachments.push(file);
                } else {
                    flowAttachments.push(file);
                }
                const attachmentItem = createAttachmentItem(file, context);
                container.appendChild(attachmentItem);
            });
        }
        
        function createAttachmentItem(file, context) {
            const div = document.createElement('div');
            div.className = 'attachment-item';
            div.innerHTML = `
                <svg class="attachment-icon" viewBox="0 0 24 24" fill="none">
                    <path d="M9 13h6m-3-3v6m5 5H7a2 2 0 01-2-2V6a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V18a2 2 0 01-2 2z" stroke-width="2"/>
                </svg>
                <span class="attachment-name">${file.name}</span>
                <span class="attachment-size">${formatFileSize(file.size)}</span>
                <div class="attachment-remove" onclick="removeAttachment('${file.name}', '${context}')">×</div>
            `;
            return div;
        }
        
        function removeAttachment(fileName, context) {
            if (context === 'main') {
                attachments = attachments.filter(f => f.name !== fileName);
                updateAttachmentsDisplay('attachmentsContainer');
            } else {
                flowAttachments = flowAttachments.filter(f => f.name !== fileName);
                updateAttachmentsDisplay('flowAttachmentsContainer');
            }
        }
        
        function updateAttachmentsDisplay(containerId) {
            const container = document.getElementById(containerId);
            container.innerHTML = '';
            const files = containerId === 'attachmentsContainer' ? attachments : flowAttachments;
            const context = containerId === 'attachmentsContainer' ? 'main' : 'flow';
            
            files.forEach(file => {
                container.appendChild(createAttachmentItem(file, context));
            });
        }
        
        function showUploadFeedback(message = 'Document uploaded successfully!') {
            const feedback = document.getElementById('uploadFeedback');
            feedback.textContent = message;
            feedback.classList.add('show');
            setTimeout(() => {
                feedback.classList.remove('show');
            }, 3000);
        }
        
        // New Session
        function createNewSession() {
            currentChatId = 'chat-' + Date.now();
            currentStep = 0;
            agentType = '';
            document.getElementById('flowContainer').innerHTML = '';
            
            // Clear input fields
            document.getElementById('mainInput').value = '';
            document.getElementById('flowInput').value = '';
            
            // Navigate to home
            navigateToHome();
            
            showUploadFeedback('New session created!');
        }
        
        // Chat History
        function toggleChatHistory() {
            const dropdown = document.getElementById('chatHistoryDropdown');
            dropdown.classList.toggle('active');
            
            document.addEventListener('click', function closeHistory(e) {
                if (!e.target.closest('.sidebar-icon')) {
                    dropdown.classList.remove('active');
                    document.removeEventListener('click', closeHistory);
                }
            });
        }
        
        function resumeChat(chatId) {
            currentChatId = chatId;
            // In a real implementation, load chat history from storage
            showUploadFeedback('Chat resumed successfully!');
            toggleChatHistory();
        }
        
        // Options dropdown
        function toggleOptionsDropdown(button) {
            const dropdown = button.querySelector('.options-dropdown');
            const allDropdowns = document.querySelectorAll('.options-dropdown');
            
            // Close all other dropdowns
            allDropdowns.forEach(d => {
                if (d !== dropdown) d.classList.remove('open');
            });
            
            dropdown.classList.toggle('open');
            
            document.addEventListener('click', function closeDropdown(e) {
                if (!e.target.closest('.action-btn')) {
                    dropdown.classList.remove('open');
                    document.removeEventListener('click', closeDropdown);
                }
            });
        }
        
        // Mode selector
        function selectMode(element, mode) {
            document.querySelectorAll('.mode-tab').forEach(tab => {
                tab.classList.remove('active');
            });
            
            element.classList.add('active');
            
            const input = document.querySelector('#mainInput');
            switch(mode) {
                case 'create':
                    input.placeholder = 'Describe the agent you want to create...';
                    break;
                case 'maintenance':
                    input.placeholder = 'What maintenance task would you like to perform?';
                    break;
                case 'general':
                    input.placeholder = 'What would you like to know?';
                    break;
            }
        }
        
        // Web search toggle
        function toggleWebSearch(button) {
            button.classList.toggle('active');
        }
        
        // Agent dropdown
        function toggleAgentDropdown(button) {
            button.classList.toggle('open');
            button.nextElementSibling.classList.toggle('open');
            
            document.addEventListener('click', function closeDropdown(e) {
                if (!e.target.closest('.agent-dropdown-container')) {
                    document.querySelectorAll('.agent-selector').forEach(sel => sel.classList.remove('open'));
                    document.querySelectorAll('.agent-dropdown').forEach(dd => dd.classList.remove('open'));
                    document.removeEventListener('click', closeDropdown);
                }
            });
        }
        
        function selectAgent(option, agentName, agentId) {
            document.querySelectorAll('.agent-name').forEach(name => {
                name.textContent = agentName;
            });
            document.querySelectorAll('.agent-option').forEach(opt => {
                opt.classList.remove('selected');
            });
            
            // Find the corresponding option in all agent dropdowns
            document.querySelectorAll('.agent-dropdown').forEach(dropdown => {
                dropdown.querySelectorAll('.agent-option').forEach(opt => {
                    const optName = opt.querySelector('.agent-option-name');
                    if (optName && optName.textContent === agentName) {
                        opt.classList.add('selected');
                    }
                });
            });
            
            document.querySelectorAll('.agent-selector').forEach(sel => sel.classList.remove('open'));
            document.querySelectorAll('.agent-dropdown').forEach(dd => dd.classList.remove('open'));
            
            console.log('Selected agent:', agentName, agentId);
        }
        
        // Start agent creation
        function startAgentCreation(type) {
            agentType = type;
            currentStep = 0;
            
            // Switch to flow view
            document.getElementById('homePage').classList.remove('active');
            document.getElementById('agentFlowPage').classList.add('active');
            document.getElementById('mainContent').classList.remove('initial');
            document.getElementById('mainContent').classList.add('conversation');
            
            // Set appropriate placeholder
            const input = document.getElementById('flowInput');
            input.placeholder = 'Describe the agent you want to create...';
            
            // Focus on input
            input.focus();
        }
        
        // Send message
        function sendMessage() {
            const isFlow = document.getElementById('agentFlowPage').classList.contains('active');
            const input = isFlow ? document.getElementById('flowInput') : document.getElementById('mainInput');
            const message = input.value.trim();
            
            if (!message) return;
            
            if (!isFlow) {
                // Start agent creation flow
                startAgentCreation('custom');
                setTimeout(() => {
                    document.getElementById('flowInput').value = message;
                    sendMessage();
                }, 100);
                return;
            }
            
            // Add step to flow
            currentStep++;
            addFlowStep(message);
            
            // Clear input and attachments
            input.value = '';
            autoResize(input);
            updateCharCount(input);
            
            flowAttachments = [];
            document.getElementById('flowAttachmentsContainer').innerHTML = '';
            
            // Simulate processing
            showProgressIndicator(currentStep);
            
            // Simulate response
            setTimeout(() => {
                hideProgressIndicator(currentStep);
                addAgentResponse(currentStep, message);
            }, 2000);
        }
        
        function addFlowStep(query) {
            const container = document.getElementById('flowContainer');
            const stepDiv = document.createElement('div');
            stepDiv.className = 'flow-step';
            stepDiv.id = `step-${currentStep}`;
            
            stepDiv.innerHTML = `
                <div class="step-header">
                    <div class="step-number">${currentStep}</div>
                    <div class="step-title">Step ${currentStep}: ${getStepTitle(currentStep)}</div>
                </div>
                <div class="step-content">
                    <div class="query-card">
                        <div class="query-label">Your Request</div>
                        <div class="query-text">${query}</div>
                    </div>
                    <div id="step-${currentStep}-response"></div>
                </div>
            `;
            
            container.appendChild(stepDiv);
            container.scrollTop = container.scrollHeight;
        }
        
        function getStepTitle(step) {
            const titles = {
                1: 'Agent Requirements',
                2: 'Capabilities Definition',
                3: 'Integration Setup',
                4: 'Testing & Deployment'
            };
            return titles[step] || `Configuration Step ${step}`;
        }
        
        function showProgressIndicator(step) {
            const responseContainer = document.getElementById(`step-${step}-response`);
            responseContainer.innerHTML = `
                <div class="progress-indicator">
                    <div class="spinner"></div>
                    Analyzing your requirements...
                </div>
            `;
        }
        
        function hideProgressIndicator(step) {
            const indicator = document.querySelector(`#step-${step}-response .progress-indicator`);
            if (indicator) {
                indicator.remove();
            }
        }
        
        function addAgentResponse(step, query) {
            const responseContainer = document.getElementById(`step-${step}-response`);
            
            // Simulate different responses based on step
            let responseContent = '';
            
            if (step === 1) {
                responseContent = `
                    <div class="result-card">
                        <div class="result-section">
                            <h4>Understanding Your Requirements</h4>
                            <p>Based on your description, I'll create an agent with the following capabilities:</p>
                            <ul class="result-list">
                                <li>Natural language understanding for user queries</li>
                                <li>Context-aware responses based on conversation history</li>
                                <li>Integration with BNY's internal knowledge base</li>
                                <li>Secure handling of sensitive financial data</li>
                            </ul>
                        </div>
                        <div class="result-section">
                            <h4>Recommended Architecture</h4>
                            <ul class="result-list">
                                <li>Base Agent: Worker Agent for optimal performance</li>
                                <li>Memory System: Vector database for context retention</li>
                                <li>Security: End-to-end encryption with BNY compliance</li>
                                <li>Response Time: &lt; 2 seconds average</li>
                            </ul>
                        </div>
                        <div class="action-buttons">
                            <button class="action-button primary" onclick="proceedToNextStep()">
                                <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                                    <path d="M5 12h14m-7-7l7 7-7 7"/>
                                </svg>
                                Continue with this setup
                            </button>
                            <button class="action-button secondary" onclick="modifyRequirements()">
                                <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                                    <path d="M11 4H4a2 2 0 00-2 2v14a2 2 0 002 2h14a2 2 0 002-2v-7"/>
                                    <path d="M18.5 2.5a2.121 2.121 0 113 3L12 15l-4 1 1-4 9.5-9.5z"/>
                                </svg>
                                Modify requirements
                            </button>
                        </div>
                    </div>
                `;
            } else if (step === 2) {
                responseContent = `
                    <div class="result-card">
                        <div class="result-section">
                            <h4>Agent Capabilities Configuration</h4>
                            <p>I've configured the following capabilities for your agent:</p>
                            <ul class="result-list">
                                <li>Multi-turn conversation handling</li>
                                <li>Document processing (PDF, DOCX, XLSX)</li>
                                <li>Real-time data analysis</li>
                                <li>API integration support</li>
                            </ul>
                        </div>
                        <div class="agent-preview">
                            <div class="agent-preview-header">
                                <div class="agent-preview-title">Your Custom Agent</div>
                                <div class="agent-status">Active</div>
                            </div>
                            <div class="result-section">
                                <h4>Preview</h4>
                                <p>Your agent is being configured with the specified capabilities. It will be able to handle various tasks based on your requirements.</p>
                            </div>
                        </div>
                        <div class="action-buttons">
                            <button class="action-button primary" onclick="deployAgent()">
                                <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                                    <path d="M21 16V8a2 2 0 00-1-1.73l-7-4a2 2 0 00-2 0l-7 4A2 2 0 003 8v8a2 2 0 001 1.73l7 4a2 2 0 002 0l7-4A2 2 0 0021 16z"/>
                                    <polyline points="3.27 6.96 12 12.01 20.73 6.96"/>
                                    <line x1="12" y1="22.08" x2="12" y2="12"/>
                                </svg>
                                Deploy Agent
                            </button>
                            <button class="action-button secondary" onclick="testAgent()">
                                <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                                    <circle cx="12" cy="12" r="10"/>
                                    <polygon points="10 8 16 12 10 16 10 8"/>
                                </svg>
                                Test Agent
                            </button>
                        </div>
                    </div>
                `;
            }
            
            responseContainer.innerHTML = responseContent;
        }
        
        function proceedToNextStep() {
            const input = document.getElementById('flowInput');
            input.placeholder = 'What specific capabilities should your agent have?';
            input.focus();
        }
        
        function modifyRequirements() {
            console.log('Modifying requirements...');
        }
        
        function deployAgent() {
            showUploadFeedback('Agent deployed successfully!');
        }
        
        function testAgent() {
            console.log('Testing agent...');
        }
        
        // Navigation
        function navigateToHome() {
            document.getElementById('agentFlowPage').classList.remove('active');
            document.getElementById('agentNotesPage').classList.remove('active');
            document.getElementById('homePage').classList.add('active');
            document.getElementById('mainContent').classList.add('initial');
            document.getElementById('mainContent').classList.remove('conversation');
            
            // Update sidebar active state
            document.querySelectorAll('.sidebar-icon').forEach(icon => {
                icon.classList.remove('active');
            });
            document.querySelector('.sidebar-icon:first-child').classList.add('active');
            
            // Reset flow
            currentStep = 0;
            agentType = '';
            document.getElementById('flowContainer').innerHTML = '';
        }
        
        // Agent Search Modal
        function openAgentSearch() {
            document.getElementById('agentSearchModal').classList.add('active');
        }
        
        function closeAgentSearch() {
            document.getElementById('agentSearchModal').classList.remove('active');
            document.getElementById('agentIdInput').value = '';
            document.getElementById('agentResult').innerHTML = '';
            document.getElementById('resultActions').style.display = 'none';
        }
        
        function searchAgent() {
            const agentId = document.getElementById('agentIdInput').value;
            if (!agentId) return;
            
            // Simulate search results
            const resultHTML = `
                <div class="result-section">
                    <h3 class="result-section-title">
                        <span class="result-badge">Agent ID: ${agentId}</span>
                        Customer Service AI Agent
                    </h3>
                    <div class="result-content">
                        This agent is designed to handle customer inquiries with advanced natural language processing capabilities.
                    </div>
                </div>
                
                <div class="result-section">
                    <h3 class="result-section-title">Core Capabilities</h3>
                    <div class="result-content">
                        • Natural language understanding and generation<br>
                        • Multi-turn conversation handling<br>
                        • Sentiment analysis and emotion detection<br>
                        • Integration with CRM systems<br>
                        • Automated ticket creation and routing
                    </div>
                </div>
                
                <div class="result-section">
                    <h3 class="result-section-title">Technical Stack</h3>
                    <div class="result-content">
                        <span class="result-badge">Python</span>
                        <span class="result-badge">TensorFlow</span>
                        <span class="result-badge">FastAPI</span>
                        <span class="result-badge">Redis</span>
                        <span class="result-badge">PostgreSQL</span>
                    </div>
                </div>
                
                <div class="result-section">
                    <h3 class="result-section-title">Performance Metrics</h3>
                    <div class="result-content">
                        • Response Time: < 200ms average<br>
                        • Accuracy: 94.5% intent recognition<br>
                        • Uptime: 99.9% SLA<br>
                        • Concurrent Users: 10,000+<br>
                        • Languages Supported: 12
                    </div>
                </div>
            `;
            
            document.getElementById('agentResult').innerHTML = resultHTML;
            document.getElementById('resultActions').style.display = 'flex';
        }
        
        function copyResults() {
            const resultText = document.getElementById('agentResult').innerText;
            navigator.clipboard.writeText(resultText);
            showUploadFeedback('Results copied to clipboard!');
        }
        
        function downloadPDF() {
            console.log('Downloading PDF...');
            showUploadFeedback('PDF downloaded successfully!');
        }
        
        // Agent Notes
        function openAgentNotes() {
            document.getElementById('homePage').classList.remove('active');
            document.getElementById('agentNotesPage').classList.add('active');
        }
        
        function saveAgentNotes() {
            const textareas = document.querySelectorAll('#agentNotesPage .notes-textarea');
            const notes = {};
            textareas.forEach((textarea) => {
                const section = textarea.closest('.notes-section').querySelector('.section-title').textContent;
                notes[section] = textarea.value;
            });
            
            console.log('Saving agent notes:', notes);
            showUploadFeedback('Agent notes saved successfully!');
        }
        
        // User Profile
        function openUserProfile() {
            document.getElementById('userProfileModal').classList.add('active');
        }
        
        function closeUserProfile() {
            document.getElementById('userProfileModal').classList.remove('active');
        }
        
        // Quick action functions
        function openAgentTemplates() {
            document.getElementById('templatesModal').classList.add('active');
        }
        
        function viewRecentAgents() {
            document.getElementById('recentAgentsModal').classList.add('active');
        }
        
        function openBNYNews() {
            window.open('https://www.bny.com/ai-news', '_blank');
        }
        
        // Sidebar functions
        function openMyAgents() {
            document.getElementById('myAgentsModal').classList.add('active');
        }
        
        function openSupport() {
            document.getElementById('supportModal').classList.add('active');
        }
        
        // Close modal functions
        function closeTemplatesModal() {
            document.getElementById('templatesModal').classList.remove('active');
        }
        
        function closeRecentAgents() {
            document.getElementById('recentAgentsModal').classList.remove('active');
        }
        
        function closeMyAgents() {
            document.getElementById('myAgentsModal').classList.remove('active');
        }
        
        function closeSupport() {
            document.getElementById('supportModal').classList.remove('active');
        }
        
        // Template functions
        function useTemplate(templateId) {
            console.log('Using template:', templateId);
            closeTemplatesModal();
            
            // Populate the input with sample text
            const sampleText = templateSamples[templateId];
            if (sampleText) {
                const input = document.getElementById('mainInput');
                input.value = sampleText;
                autoResize(input);
                updateCharCount(input);
                
                // Focus on input
                input.focus();
                
                showUploadFeedback('Template loaded! Feel free to modify the description.');
            }
        }
        
        // Agent functions
        function loadAgent(agentId) {
            console.log('Loading agent:', agentId);
            closeRecentAgents();
            showUploadFeedback('Agent loaded successfully!');
        }
        
        function setTheme(theme) {
            document.querySelectorAll('.theme-option').forEach(opt => {
                opt.classList.remove('active');
            });
            
            if (theme === 'dark') {
                document.body.classList.add('dark');
                document.querySelectorAll('.theme-option').forEach(opt => {
                    if (opt.textContent.trim() === 'Dark') {
                        opt.classList.add('active');
                    }
                });
            } else {
                document.body.classList.remove('dark');
                document.querySelectorAll('.theme-option').forEach(opt => {
                    if (opt.textContent.trim() === 'Light') {
                        opt.classList.add('active');
                    }
                });
            }
            
            localStorage.setItem('theme', theme);
        }
        
        // Load saved theme
        const savedTheme = localStorage.getItem('theme') || 'light';
        setTheme(savedTheme);
        
        // Close modals on overlay click
        document.querySelectorAll('.modal-overlay').forEach(modal => {
            modal.addEventListener('click', function(e) {
                if (e.target === this) {
                    this.classList.remove('active');
                }
            });
        });
    </script>
</body>
</html>
