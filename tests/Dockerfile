# שימוש בתמונה בסיסית של פייתון
FROM python:3.9-slim

# הגדרת תיקיית עבודה בתוך האימג'
WORKDIR /app

# העתקת קובץ הדרישות והתקנתן
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# העתקת שאר קבצי הקוד (כמו api.py ו-calculator_app.py)
COPY . .

# הפקודה שתריץ את האפליקציה (בהנחה ש-api.py הוא הקובץ הראשי)
CMD ["python", "api.py"]
