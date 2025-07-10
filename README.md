import { useSearchParams } from "next/navigation";
import { useEffect, useState } from "react";
import { CheckCircle, XCircle } from "lucide-react";

const validIds = new Set(
  Array.from({ length: 200 }, (_, i) => `WD-${String(i + 1).padStart(3, "0")}`)
);

export default function VerifyPage() {
  const searchParams = useSearchParams();
  const id = searchParams.get("id");
  const [status, setStatus] = useState<"valid" | "invalid" | "none">("none");

  useEffect(() => {
    if (id) {
      setStatus(validIds.has(id) ? "valid" : "invalid");
    }
  }, [id]);

  return (
    <div className="min-h-screen flex items-center justify-center bg-gray-100">
      <div className="bg-white p-6 rounded-2xl shadow-xl text-center max-w-sm w-full">
        {status === "none" && <p className="text-lg">Oczekiwanie na dane...</p>}

        {status === "valid" && (
          <div>
            <CheckCircle className="w-16 h-16 text-green-500 mx-auto mb-4" />
            <h1 className="text-2xl font-bold mb-2">Pojazd potwierdzony</h1>
            <p className="text-gray-600">ID: {id} należy do floty partnera.</p>
          </div>
        )}

        {status === "invalid" && (
          <div>
            <XCircle className="w-16 h-16 text-red-500 mx-auto mb-4" />
            <h1 className="text-2xl font-bold mb-2">Nieznany pojazd</h1>
            <p className="text-gray-600">ID: {id} nie znajduje się w bazie.</p>
          </div>
        )}
      </div>
    </div>
  );
}
